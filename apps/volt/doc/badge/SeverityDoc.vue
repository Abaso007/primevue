<template>
    <DocSectionText v-bind="$attrs">
        <p>Severity defines the variant of a badge.</p>
    </DocSectionText>
    <div class="card flex flex-wrap justify-center gap-2">
        <Badge value="2"></Badge>
        <Badge value="6" severity="secondary"></Badge>
        <Badge value="8" severity="success"></Badge>
        <Badge value="4" severity="info"></Badge>
        <Badge value="9" severity="warn"></Badge>
        <Badge value="3" severity="danger"></Badge>
        <Badge value="5" severity="contrast"></Badge>
    </div>
    <DocSectionCode :code="code" />
</template>

<script setup lang="ts">
import Badge from '@/volt/Badge.vue';
import { ref } from 'vue';

const code = ref(`
<template>
    <div class="card flex flex-wrap justify-center gap-2">
        <Badge value="2"></Badge>
        <Badge value="6" severity="secondary"></Badge>
        <Badge value="8" severity="success"></Badge>
        <Badge value="4" severity="info"></Badge>
        <Badge value="9" severity="warn"></Badge>
        <Badge value="3" severity="danger"></Badge>
        <Badge value="5" severity="contrast"></Badge>
    </div>
</template>

<script setup lang="ts">
import Badge from '@/volt/Badge.vue';
<\/script>
`);
</script>
