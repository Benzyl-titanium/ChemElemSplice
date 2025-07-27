<template>
  <div>
    <ElementInput v-model="customOrder" v-model:rawInput="rawInput" />
    <ElementTable :customOrder="customOrder" :rawInput="rawInput" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import ElementTable from './components/ElementTable.vue';
import ElementInput from './components/ElementInput.vue';

const customOrder = ref<(number | string)[]>([]);
const rawInput = ref('');
</script> 