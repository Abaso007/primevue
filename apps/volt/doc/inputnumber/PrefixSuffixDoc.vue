<template>
    <DocSectionText v-bind="$attrs">
        <p>Custom texts e.g. units can be placed before or after the input section with the <i>prefix</i> and <i>suffix</i> properties.</p>
    </DocSectionText>
    <div class="card flex flex-wrap gap-4">
        <div class="flex-auto">
            <label for="mile" class="font-bold block mb-2"> Mile </label>
            <InputNumber v-model="value1" inputId="mile" suffix=" mi" fluid />
        </div>
        <div class="flex-auto">
            <label for="percent" class="font-bold block mb-2"> Percent </label>
            <InputNumber v-model="value2" inputId="percent" prefix="%" fluid />
        </div>
        <div class="flex-auto">
            <label for="expiry" class="font-bold block mb-2"> Expiry </label>
            <InputNumber v-model="value3" inputId="expiry" prefix="Expires in " suffix=" days" fluid />
        </div>
        <div class="flex-auto">
            <label for="temperature" class="font-bold block mb-2"> Temperature </label>
            <InputNumber v-model="value4" inputId="temperature" prefix="&uarr; " suffix="℃" :min="0" :max="40" fluid />
        </div>
    </div>
    <DocSectionCode :code="code" />
</template>

<script setup lang="ts">
import InputNumber from '@/volt/InputNumber.vue';
import { ref } from 'vue';

const value1 = ref(20);
const value2 = ref(50);
const value3 = ref(10);
const value4 = ref(20);

const code = ref(`
<template>
    <div class="card flex flex-wrap gap-4">
        <div class="flex-auto">
            <label for="mile" class="font-bold block mb-2"> Mile </label>
            <InputNumber v-model="value1" inputId="mile" suffix=" mi" fluid />
        </div>
        <div class="flex-auto">
            <label for="percent" class="font-bold block mb-2"> Percent </label>
            <InputNumber v-model="value2" inputId="percent" prefix="%" fluid />
        </div>
        <div class="flex-auto">
            <label for="expiry" class="font-bold block mb-2"> Expiry </label>
            <InputNumber v-model="value3" inputId="expiry" prefix="Expires in " suffix=" days" fluid />
        </div>
        <div class="flex-auto">
            <label for="temperature" class="font-bold block mb-2"> Temperature </label>
            <InputNumber v-model="value4" inputId="temperature" prefix="&uarr; " suffix="℃" :min="0" :max="40" fluid />
        </div>
    </div>
</template>

<script setup lang="ts">
import InputNumber from '@/volt/InputNumber.vue';
import { ref } from 'vue';

const value1 = ref(20);
const value2 = ref(50);
const value3 = ref(10);
const value4 = ref(20);
<\/script>
`);
</script>
