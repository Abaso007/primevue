<template>
    <DocComponent title="Vue Otp Input Component" header="InputOtp" description="Input Otp is used to enter one time passwords." :componentDocs="docs" />
</template>

<script setup lang="ts">
import BasicDoc from '@/doc/inputotp/BasicDoc.vue';
import DownloadDoc from '@/doc/inputotp/DownloadDoc.vue';
import FilledDoc from '@/doc/inputotp/FilledDoc.vue';
import ImportDoc from '@/doc/inputotp/ImportDoc.vue';
import IntegerOnlyDoc from '@/doc/inputotp/IntegerOnlyDoc.vue';
import MaskDoc from '@/doc/inputotp/MaskDoc.vue';
import SampleDoc from '@/doc/inputotp/SampleDoc.vue';
import SizesDoc from '@/doc/inputotp/SizesDoc.vue';
import TemplateDoc from '@/doc/inputotp/TemplateDoc.vue';
import { ref } from 'vue';

const docs = ref([
    {
        id: 'dowload',
        label: 'Download',
        component: DownloadDoc
    },
    {
        id: 'import',
        label: 'Import',
        component: ImportDoc
    },
    {
        id: 'basic',
        label: 'Basic',
        component: BasicDoc
    },
    {
        id: 'mask',
        label: 'Mask',
        component: MaskDoc
    },
    {
        id: 'integeronly',
        label: 'Integer Only',
        component: IntegerOnlyDoc
    },
    {
        id: 'filled',
        label: 'Filled',
        component: FilledDoc
    },
    {
        id: 'sizes',
        label: 'Sizes',
        component: SizesDoc
    },
    {
        id: 'template',
        label: 'Template',
        component: TemplateDoc
    },
    {
        id: 'sample',
        label: 'Sample',
        component: SampleDoc
    }
]);
</script>
