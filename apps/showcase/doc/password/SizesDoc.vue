<template>
    <DocSectionText v-bind="$attrs">
        <p>Password provides <i>small</i> and <i>large</i> sizes as alternatives to the base.</p>
    </DocSectionText>
    <div class="card flex flex-col items-center gap-4">
        <Password v-model="value1" size="small" placeholder="Small" />
        <Password v-model="value2" placeholder="Normal" />
        <Password v-model="value3" size="large" placeholder="Large" />
    </div>
    <DocSectionCode :code="code" />
</template>

<script>
export default {
    data() {
        return {
            value1: null,
            value2: null,
            value3: null,
            code: {
                basic: `
<div class="card flex flex-col items-center gap-4">
    <Password v-model="value1" size="small" placeholder="Small" />
    <Password v-model="value2" placeholder="Normal" />
    <Password v-model="value3" size="large" placeholder="Large" />
</div>
`,
                options: `
<template>
    <div class="card flex flex-col items-center gap-4">
        <Password v-model="value1" size="small" placeholder="Small" />
        <Password v-model="value2" placeholder="Normal" />
        <Password v-model="value3" size="large" placeholder="Large" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            value1: null,
            value2: null,
            value3: null
        }
    }
}
<\/script>
`,
                composition: `
<template>
    <div class="card flex flex-col items-center gap-4">
        <Password v-model="value1" size="small" placeholder="Small" />
        <Password v-model="value2" placeholder="Normal" />
        <Password v-model="value3" size="large" placeholder="Large" />
    </div>
</template>

<script setup>
import { ref } from 'vue';

const value1 = ref(null);
const value2 = ref(null);
const value3 = ref(null);
<\/script>
`
            }
        };
    }
};
</script>
