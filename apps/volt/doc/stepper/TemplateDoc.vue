<template>
    <DocSectionText v-bind="$attrs">
        <p>
            Custom content for a step is defined with the default slot. The optional <i>as</i> property controls the default container element of a step, for example setting it to a <i>button</i> renders a button for the header instead of a div. The
            <i>asChild</i> option enables the headless mode for further customization by passing callbacks and properties to implement your own step.
        </p>
    </DocSectionText>
    <div class="card flex justify-center">
        <Stepper v-model:value="activeStep" class="basis-[40rem]">
            <StepList>
                <Step v-slot="{ activateCallback, value, a11yAttrs }" asChild :value="1">
                    <div class="flex flex-row flex-auto gap-2" v-bind="a11yAttrs.root">
                        <button class="bg-transparent border-0 inline-flex flex-col gap-2" @click="activateCallback" v-bind="a11yAttrs.header">
                            <span
                                :class="[
                                    'rounded-full border-2 w-12 h-12 inline-flex items-center justify-center',
                                    { 'bg-primary text-primary-contrast border-primary': value <= activeStep, 'border-surface-200 dark:border-surface-700': value > activeStep }
                                ]"
                            >
                                <i class="pi pi-user" />
                            </span>
                        </button>
                        <Divider />
                    </div>
                </Step>
                <Step v-slot="{ activateCallback, value, a11yAttrs }" asChild :value="2">
                    <div class="flex flex-row flex-auto gap-2 pl-2" v-bind="a11yAttrs.root">
                        <button class="bg-transparent border-0 inline-flex flex-col gap-2" @click="activateCallback" v-bind="a11yAttrs.header">
                            <span
                                :class="[
                                    'rounded-full border-2 w-12 h-12 inline-flex items-center justify-center',
                                    { 'bg-primary text-primary-contrast border-primary': value <= activeStep, 'border-surface-200 dark:border-surface-700': value > activeStep }
                                ]"
                            >
                                <i class="pi pi-star" />
                            </span>
                        </button>
                        <Divider />
                    </div>
                </Step>
                <Step v-slot="{ activateCallback, value, a11yAttrs }" asChild :value="3">
                    <div class="flex flex-row pl-2" v-bind="a11yAttrs.root">
                        <button class="bg-transparent border-0 inline-flex flex-col gap-2" @click="activateCallback" v-bind="a11yAttrs.header">
                            <span
                                :class="[
                                    'rounded-full border-2 w-12 h-12 inline-flex items-center justify-center',
                                    { 'bg-primary text-primary-contrast border-primary': value <= activeStep, 'border-surface-200 dark:border-surface-700': value > activeStep }
                                ]"
                            >
                                <i class="pi pi-id-card" />
                            </span>
                        </button>
                    </div>
                </Step>
            </StepList>
            <StepPanels>
                <StepPanel v-slot="{ activateCallback }" :value="1">
                    <form @submit.prevent>
                        <div class="flex flex-col gap-4 mx-auto" style="min-height: 16rem; max-width: 20rem">
                            <div class="text-center mt-4 mb-4 text-xl font-semibold">Create your account</div>
                            <div class="field">
                                <InputText v-model="name" type="text" placeholder="Name" fluid autocomplete="off" />
                            </div>
                            <div class="field">
                                <InputText v-model="email" type="email" placeholder="Email" fluid autocomplete="off" />
                            </div>
                            <div class="field">
                                <Password v-model="password" placeholder="Password" fluid :inputProps="{ autocomplete: 'new-password' }" />
                            </div>
                        </div>
                        <div class="flex pt-6 justify-end">
                            <Button label="Next" icon="pi pi-arrow-right" iconPos="right" @click="activateCallback(2)" />
                        </div>
                    </form>
                </StepPanel>
                <StepPanel v-slot="{ activateCallback }" :value="2">
                    <div class="flex flex-col gap-2 mx-auto" style="min-height: 16rem; max-width: 24rem">
                        <div class="text-center mt-4 mb-4 text-xl font-semibold">Choose your interests</div>
                        <div class="flex flex-wrap justify-center gap-4">
                            <ToggleButton v-model="option1" onLabel="Nature" offLabel="Nature" />
                            <ToggleButton v-model="option2" onLabel="Art" offLabel="Art" />
                            <ToggleButton v-model="option3" onLabel="Music" offLabel="Music" />
                            <ToggleButton v-model="option4" onLabel="Design" offLabel="Design" />
                            <ToggleButton v-model="option5" onLabel="Photography" offLabel="Photography" />
                            <ToggleButton v-model="option6" onLabel="Movies" offLabel="Movies" />
                            <ToggleButton v-model="option7" onLabel="Sports" offLabel="Sports" />
                            <ToggleButton v-model="option8" onLabel="Gaming" offLabel="Gaming" />
                            <ToggleButton v-model="option9" onLabel="Traveling" offLabel="Traveling" />
                            <ToggleButton v-model="option10" onLabel="Dancing" offLabel="Dancing" />
                        </div>
                    </div>
                    <div class="flex pt-6 justify-between">
                        <SecondaryButton label="Back" icon="pi pi-arrow-left" @click="activateCallback(1)" />
                        <Button label="Next" icon="pi pi-arrow-right" iconPos="right" @click="activateCallback(3)" />
                    </div>
                </StepPanel>
                <StepPanel v-slot="{ activateCallback }" :value="3">
                    <div class="flex flex-col gap-2 mx-auto" style="min-height: 16rem; max-width: 24rem">
                        <div class="text-center mt-4 mb-4 text-xl font-semibold">Account created successfully</div>
                        <div class="flex justify-center">
                            <img alt="logo" src="https://primefaces.org/cdn/primevue/images/stepper/content.svg" />
                        </div>
                    </div>
                    <div class="flex pt-6 justify-start">
                        <SecondaryButton label="Back" icon="pi pi-arrow-left" @click="activateCallback(2)" />
                    </div>
                </StepPanel>
            </StepPanels>
        </Stepper>
    </div>
    <DocSectionCode :code="code" />
</template>

<script setup lang="ts">
import Button from '@/volt/Button.vue';
import Divider from '@/volt/Divider.vue';
import InputText from '@/volt/InputText.vue';
import Password from '@/volt/Password.vue';
import SecondaryButton from '@/volt/SecondaryButton.vue';
import Step from '@/volt/Step.vue';
import StepList from '@/volt/StepList.vue';
import StepPanel from '@/volt/StepPanel.vue';
import StepPanels from '@/volt/StepPanels.vue';
import Stepper from '@/volt/Stepper.vue';
import ToggleButton from '@/volt/ToggleButton.vue';
import { ref } from 'vue';

const activeStep = ref(1);
const name = ref();
const email = ref();
const password = ref();
const option1 = ref(false);
const option2 = ref(false);
const option3 = ref(false);
const option4 = ref(false);
const option5 = ref(false);
const option6 = ref(false);
const option7 = ref(false);
const option8 = ref(false);
const option9 = ref(false);
const option10 = ref(false);

const code = ref(`
<template>
    <div class="card flex justify-center">
        <Stepper v-model:value="activeStep" class="basis-[40rem]">
            <StepList>
                <Step v-slot="{ activateCallback, value, a11yAttrs }" asChild :value="1">
                    <div class="flex flex-row flex-auto gap-2" v-bind="a11yAttrs.root">
                        <button class="bg-transparent border-0 inline-flex flex-col gap-2" @click="activateCallback" v-bind="a11yAttrs.header">
                            <span
                                :class="[
                                    'rounded-full border-2 w-12 h-12 inline-flex items-center justify-center',
                                    { 'bg-primary text-primary-contrast border-primary': value <= activeStep, 'border-surface-200 dark:border-surface-700': value > activeStep }
                                ]"
                            >
                                <i class="pi pi-user" />
                            </span>
                        </button>
                        <Divider />
                    </div>
                </Step>
                <Step v-slot="{ activateCallback, value, a11yAttrs }" asChild :value="2">
                    <div class="flex flex-row flex-auto gap-2 pl-2" v-bind="a11yAttrs.root">
                        <button class="bg-transparent border-0 inline-flex flex-col gap-2" @click="activateCallback" v-bind="a11yAttrs.header">
                            <span
                                :class="[
                                    'rounded-full border-2 w-12 h-12 inline-flex items-center justify-center',
                                    { 'bg-primary text-primary-contrast border-primary': value <= activeStep, 'border-surface-200 dark:border-surface-700': value > activeStep }
                                ]"
                            >
                                <i class="pi pi-star" />
                            </span>
                        </button>
                        <Divider />
                    </div>
                </Step>
                <Step v-slot="{ activateCallback, value, a11yAttrs }" asChild :value="3">
                    <div class="flex flex-row pl-2" v-bind="a11yAttrs.root">
                        <button class="bg-transparent border-0 inline-flex flex-col gap-2" @click="activateCallback" v-bind="a11yAttrs.header">
                            <span
                                :class="[
                                    'rounded-full border-2 w-12 h-12 inline-flex items-center justify-center',
                                    { 'bg-primary text-primary-contrast border-primary': value <= activeStep, 'border-surface-200 dark:border-surface-700': value > activeStep }
                                ]"
                            >
                                <i class="pi pi-id-card" />
                            </span>
                        </button>
                    </div>
                </Step>
            </StepList>
            <StepPanels>
                <StepPanel v-slot="{ activateCallback }" :value="1">
                    <form @submit.prevent>
                        <div class="flex flex-col gap-4 mx-auto" style="min-height: 16rem; max-width: 20rem">
                            <div class="text-center mt-4 mb-4 text-xl font-semibold">Create your account</div>
                            <div class="field">
                                <InputText v-model="name" type="text" placeholder="Name" fluid autocomplete="off" />
                            </div>
                            <div class="field">
                                <InputText v-model="email" type="email" placeholder="Email" fluid autocomplete="off" />
                            </div>
                            <div class="field">
                                <Password v-model="password" placeholder="Password" fluid :inputProps="{ autocomplete: 'new-password' }" />
                            </div>
                        </div>
                        <div class="flex pt-6 justify-end">
                            <Button label="Next" icon="pi pi-arrow-right" iconPos="right" @click="activateCallback(2)" />
                        </div>
                    </form>
                </StepPanel>
                <StepPanel v-slot="{ activateCallback }" :value="2">
                    <div class="flex flex-col gap-2 mx-auto" style="min-height: 16rem; max-width: 24rem">
                        <div class="text-center mt-4 mb-4 text-xl font-semibold">Choose your interests</div>
                        <div class="flex flex-wrap justify-center gap-4">
                            <ToggleButton v-model="option1" onLabel="Nature" offLabel="Nature" />
                            <ToggleButton v-model="option2" onLabel="Art" offLabel="Art" />
                            <ToggleButton v-model="option3" onLabel="Music" offLabel="Music" />
                            <ToggleButton v-model="option4" onLabel="Design" offLabel="Design" />
                            <ToggleButton v-model="option5" onLabel="Photography" offLabel="Photography" />
                            <ToggleButton v-model="option6" onLabel="Movies" offLabel="Movies" />
                            <ToggleButton v-model="option7" onLabel="Sports" offLabel="Sports" />
                            <ToggleButton v-model="option8" onLabel="Gaming" offLabel="Gaming" />
                            <ToggleButton v-model="option9" onLabel="Traveling" offLabel="Traveling" />
                            <ToggleButton v-model="option10" onLabel="Dancing" offLabel="Dancing" />
                        </div>
                    </div>
                    <div class="flex pt-6 justify-between">
                        <SecondaryButton label="Back" icon="pi pi-arrow-left" @click="activateCallback(1)" />
                        <Button label="Next" icon="pi pi-arrow-right" iconPos="right" @click="activateCallback(3)" />
                    </div>
                </StepPanel>
                <StepPanel v-slot="{ activateCallback }" :value="3">
                    <div class="flex flex-col gap-2 mx-auto" style="min-height: 16rem; max-width: 24rem">
                        <div class="text-center mt-4 mb-4 text-xl font-semibold">Account created successfully</div>
                        <div class="flex justify-center">
                            <img alt="logo" src="https://primefaces.org/cdn/primevue/images/stepper/content.svg" />
                        </div>
                    </div>
                    <div class="flex pt-6 justify-start">
                        <SecondaryButton label="Back" icon="pi pi-arrow-left" @click="activateCallback(2)" />
                    </div>
                </StepPanel>
            </StepPanels>
        </Stepper>
    </div>
</template>

<script setup lang="ts">
import Button from '@/volt/Button.vue';
import SecondaryButton from '@/volt/SecondaryButton.vue';
import Divider from '@/volt/Divider.vue';
import InputText from '@/volt/InputText.vue';
import Password from '@/volt/Password.vue';
import Stepper from '@/volt/Stepper.vue';
import Step from '@/volt/Step.vue';
import StepList from '@/volt/StepList.vue';
import StepPanel from '@/volt/StepPanel.vue';
import StepPanels from '@/volt/StepPanels.vue';
import ToggleButton from '@/volt/ToggleButton.vue';
import { ref } from 'vue';

const activeStep = ref(1);
const name = ref();
const email = ref();
const password = ref();
const option1 = ref(false);
const option2 = ref(false);
const option3 = ref(false);
const option4 = ref(false);
const option5 = ref(false);
const option6 = ref(false);
const option7 = ref(false);
const option8 = ref(false);
const option9 = ref(false);
const option10 = ref(false);
<\/script>
`);
</script>
