<template>
    <Portal :appendTo="appendTo" :disabled="!popup">
        <transition name="p-connected-overlay" @enter="onEnter" @leave="onLeave" @after-leave="onAfterLeave" v-bind="ptm('transition')">
            <div v-if="popup ? overlayVisible : true" :ref="containerRef" :id="$id" :class="cx('root')" @click="onOverlayClick" :data-p="dataP" v-bind="ptmi('root')">
                <div v-if="$slots.start" :class="cx('start')" v-bind="ptm('start')">
                    <slot name="start"></slot>
                </div>
                <ul
                    :ref="listRef"
                    :id="$id + '_list'"
                    :class="cx('list')"
                    role="menu"
                    :tabindex="tabindex"
                    :aria-activedescendant="focused ? focusedOptionId : undefined"
                    :aria-label="ariaLabel"
                    :aria-labelledby="ariaLabelledby"
                    @focus="onListFocus"
                    @blur="onListBlur"
                    @keydown="onListKeyDown"
                    v-bind="ptm('list')"
                >
                    <template v-for="(item, i) of model" :key="label(item) + i.toString()">
                        <template v-if="item.items && visible(item) && !item.separator">
                            <li v-if="item.items" :id="$id + '_' + i" :class="[cx('submenuLabel'), item.class]" role="none" v-bind="ptm('submenuLabel')">
                                <!--TODO: submenuheader deprecated since v4.0. Use submenulabel-->
                                <slot :name="$slots.submenulabel ? 'submenulabel' : 'submenuheader'" :item="item">{{ label(item) }}</slot>
                            </li>
                            <template v-for="(child, j) of item.items" :key="child.label + i + '_' + j">
                                <PVMenuitem
                                    v-if="visible(child) && !child.separator"
                                    :id="$id + '_' + i + '_' + j"
                                    :item="child"
                                    :templates="$slots"
                                    :focusedOptionId="focusedOptionId"
                                    :unstyled="unstyled"
                                    @item-click="itemClick"
                                    @item-mousemove="itemMouseMove"
                                    :pt="pt"
                                />
                                <li v-else-if="visible(child) && child.separator" :key="'separator' + i + j" :class="[cx('separator'), item.class]" :style="child.style" role="separator" v-bind="ptm('separator')"></li>
                            </template>
                        </template>
                        <li v-else-if="visible(item) && item.separator" :key="'separator' + i.toString()" :class="[cx('separator'), item.class]" :style="item.style" role="separator" v-bind="ptm('separator')"></li>
                        <PVMenuitem
                            v-else
                            :key="label(item) + i.toString()"
                            :id="$id + '_' + i"
                            :item="item"
                            :index="i"
                            :templates="$slots"
                            :focusedOptionId="focusedOptionId"
                            :unstyled="unstyled"
                            @item-click="itemClick"
                            @item-mousemove="itemMouseMove"
                            :pt="pt"
                        />
                    </template>
                </ul>
                <div v-if="$slots.end" :class="cx('end')" v-bind="ptm('end')">
                    <slot name="end"></slot>
                </div>
            </div>
        </transition>
    </Portal>
</template>

<script>
import { cn } from '@primeuix/utils';
import { absolutePosition, addStyle, find, findSingle, focus, getOuterWidth, isTouchDevice } from '@primeuix/utils/dom';
import { ZIndex } from '@primeuix/utils/zindex';
import { ConnectedOverlayScrollHandler } from '@primevue/core/utils';
import OverlayEventBus from 'primevue/overlayeventbus';
import Portal from 'primevue/portal';
import BaseMenu from './BaseMenu.vue';
import Menuitem from './Menuitem.vue';

export default {
    name: 'Menu',
    extends: BaseMenu,
    inheritAttrs: false,
    emits: ['show', 'hide', 'focus', 'blur'],
    data() {
        return {
            overlayVisible: false,
            focused: false,
            focusedOptionIndex: -1,
            selectedOptionIndex: -1
        };
    },
    target: null,
    outsideClickListener: null,
    scrollHandler: null,
    resizeListener: null,
    container: null,
    list: null,
    mounted() {
        if (!this.popup) {
            this.bindResizeListener();
            this.bindOutsideClickListener();
        }
    },
    beforeUnmount() {
        this.unbindResizeListener();
        this.unbindOutsideClickListener();

        if (this.scrollHandler) {
            this.scrollHandler.destroy();
            this.scrollHandler = null;
        }

        this.target = null;

        if (this.container && this.autoZIndex) {
            ZIndex.clear(this.container);
        }

        this.container = null;
    },
    methods: {
        itemClick(event) {
            const item = event.item;

            if (this.disabled(item)) {
                return;
            }

            if (this.overlayVisible) this.hide();

            if (item.command) {
                item.command(event);
            }

            if (!this.popup && this.focusedOptionIndex !== event.id) {
                this.focusedOptionIndex = event.id;
            }
        },
        itemMouseMove(event) {
            if (this.focused) {
                this.focusedOptionIndex = event.id;
            }
        },
        onListFocus(event) {
            this.focused = true;
            !this.popup && this.changeFocusedOptionIndex(0);

            this.$emit('focus', event);
        },
        onListBlur(event) {
            this.focused = false;
            this.focusedOptionIndex = -1;
            this.$emit('blur', event);
        },
        onListKeyDown(event) {
            switch (event.code) {
                case 'ArrowDown':
                    this.onArrowDownKey(event);
                    break;

                case 'ArrowUp':
                    this.onArrowUpKey(event);
                    break;

                case 'Home':
                    this.onHomeKey(event);
                    break;

                case 'End':
                    this.onEndKey(event);
                    break;

                case 'Enter':
                case 'NumpadEnter':
                    this.onEnterKey(event);
                    break;

                case 'Space':
                    this.onSpaceKey(event);
                    break;

                case 'Escape':
                    if (this.popup) {
                        focus(this.target);
                        this.hide();
                    }

                case 'Tab':
                    this.overlayVisible && this.hide();
                    break;

                default:
                    break;
            }
        },
        onArrowDownKey(event) {
            const optionIndex = this.findNextOptionIndex(this.focusedOptionIndex);

            this.changeFocusedOptionIndex(optionIndex);
            event.preventDefault();
        },
        onArrowUpKey(event) {
            if (event.altKey && this.popup) {
                focus(this.target);
                this.hide();
                event.preventDefault();
            } else {
                const optionIndex = this.findPrevOptionIndex(this.focusedOptionIndex);

                this.changeFocusedOptionIndex(optionIndex);
                event.preventDefault();
            }
        },
        onHomeKey(event) {
            this.changeFocusedOptionIndex(0);
            event.preventDefault();
        },
        onEndKey(event) {
            this.changeFocusedOptionIndex(find(this.container, 'li[data-pc-section="item"][data-p-disabled="false"]').length - 1);
            event.preventDefault();
        },
        onEnterKey(event) {
            const element = findSingle(this.list, `li[id="${`${this.focusedOptionIndex}`}"]`);
            const anchorElement = element && findSingle(element, 'a[data-pc-section="itemlink"]');

            this.popup && focus(this.target);
            anchorElement ? anchorElement.click() : element && element.click();

            event.preventDefault();
        },
        onSpaceKey(event) {
            this.onEnterKey(event);
        },
        findNextOptionIndex(index) {
            const links = find(this.container, 'li[data-pc-section="item"][data-p-disabled="false"]');
            const matchedOptionIndex = [...links].findIndex((link) => link.id === index);

            return matchedOptionIndex > -1 ? matchedOptionIndex + 1 : 0;
        },
        findPrevOptionIndex(index) {
            const links = find(this.container, 'li[data-pc-section="item"][data-p-disabled="false"]');
            const matchedOptionIndex = [...links].findIndex((link) => link.id === index);

            return matchedOptionIndex > -1 ? matchedOptionIndex - 1 : 0;
        },
        changeFocusedOptionIndex(index) {
            const links = find(this.container, 'li[data-pc-section="item"][data-p-disabled="false"]');
            let order = index >= links.length ? links.length - 1 : index < 0 ? 0 : index;

            order > -1 && (this.focusedOptionIndex = links[order].getAttribute('id'));
        },
        toggle(event, target) {
            if (this.overlayVisible) this.hide();
            else this.show(event, target);
        },
        show(event, target) {
            this.overlayVisible = true;
            this.target = target ?? event.currentTarget;
        },
        hide() {
            this.overlayVisible = false;
            this.target = null;
        },
        onEnter(el) {
            addStyle(el, { position: 'absolute', top: '0' });
            this.alignOverlay();
            this.bindOutsideClickListener();
            this.bindResizeListener();
            this.bindScrollListener();

            if (this.autoZIndex) {
                ZIndex.set('menu', el, this.baseZIndex + this.$primevue.config.zIndex.menu);
            }

            if (this.popup) {
                focus(this.list);
            }

            this.$emit('show');
        },
        onLeave() {
            this.unbindOutsideClickListener();
            this.unbindResizeListener();
            this.unbindScrollListener();
            this.$emit('hide');
        },
        onAfterLeave(el) {
            if (this.autoZIndex) {
                ZIndex.clear(el);
            }
        },
        alignOverlay() {
            absolutePosition(this.container, this.target);
            const targetWidth = getOuterWidth(this.target);

            if (targetWidth > getOuterWidth(this.container)) {
                this.container.style.minWidth = getOuterWidth(this.target) + 'px';
            }
        },
        bindOutsideClickListener() {
            if (!this.outsideClickListener) {
                this.outsideClickListener = (event) => {
                    const isOutsideContainer = this.container && !this.container.contains(event.target);
                    const isOutsideTarget = !(this.target && (this.target === event.target || this.target.contains(event.target)));

                    if (this.overlayVisible && isOutsideContainer && isOutsideTarget) {
                        this.hide();
                    } else if (!this.popup && isOutsideContainer && isOutsideTarget) {
                        this.focusedOptionIndex = -1;
                    }
                };

                document.addEventListener('click', this.outsideClickListener, true);
            }
        },
        unbindOutsideClickListener() {
            if (this.outsideClickListener) {
                document.removeEventListener('click', this.outsideClickListener, true);
                this.outsideClickListener = null;
            }
        },
        bindScrollListener() {
            if (!this.scrollHandler) {
                this.scrollHandler = new ConnectedOverlayScrollHandler(this.target, () => {
                    if (this.overlayVisible) {
                        this.hide();
                    }
                });
            }

            this.scrollHandler.bindScrollListener();
        },
        unbindScrollListener() {
            if (this.scrollHandler) {
                this.scrollHandler.unbindScrollListener();
            }
        },
        bindResizeListener() {
            if (!this.resizeListener) {
                this.resizeListener = () => {
                    if (this.overlayVisible && !isTouchDevice()) {
                        this.hide();
                    }
                };

                window.addEventListener('resize', this.resizeListener);
            }
        },
        unbindResizeListener() {
            if (this.resizeListener) {
                window.removeEventListener('resize', this.resizeListener);
                this.resizeListener = null;
            }
        },
        visible(item) {
            return typeof item.visible === 'function' ? item.visible() : item.visible !== false;
        },
        disabled(item) {
            return typeof item.disabled === 'function' ? item.disabled() : item.disabled;
        },
        label(item) {
            return typeof item.label === 'function' ? item.label() : item.label;
        },
        onOverlayClick(event) {
            OverlayEventBus.emit('overlay-click', {
                originalEvent: event,
                target: this.target
            });
        },
        containerRef(el) {
            this.container = el;
        },
        listRef(el) {
            this.list = el;
        }
    },
    computed: {
        focusedOptionId() {
            return this.focusedOptionIndex !== -1 ? this.focusedOptionIndex : null;
        },
        dataP() {
            return cn({
                popup: this.popup
            });
        }
    },
    components: {
        PVMenuitem: Menuitem,
        Portal: Portal
    }
};
</script>
