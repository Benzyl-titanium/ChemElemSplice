<template>
  <div>
    <div v-if="orderedElements.length === 0 && props.rawInput && props.rawInput.trim() !== ''" style="color: #fff; text-align: center; margin: 24px 0; font-size: 18px;">
      无法用元素符号拼写 "{{ props.rawInput }}"
    </div>
    <div ref="tableRef" class="element-table" v-if="orderedElements.length > 0">
      <ElementBlock
        v-for="(el, idx) in orderedElements"
        :key="el.number"
        :element="el"
        :isFirst="idx === 0"
        :isLast="idx === orderedElements.length - 1"
      />
    </div>
    <div style="margin-top: 16px;" v-if="orderedElements.length > 0">
      <button @click="exportPNG">PNG</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue';
import ElementBlock from './ElementBlock.vue';
import elementsData from '../assets/elements.json';
import html2canvas from 'html2canvas';
// @ts-ignore
import domtoimage from 'dom-to-image-more';

interface Element {
  symbol: string;
  name: string;
  number: number;
  mass: number | null;
}

const elements: Element[] = elementsData.elements;

const props = defineProps<{
  customOrder: (number | string)[],
  rawInput?: string
}>();

const tableRef = ref<HTMLElement | null>(null);

const orderedElements = computed(() => {
  return props.customOrder.map((item: number | string) => {
    if (typeof item === 'string') {
      return elements.find((e: Element) => e.symbol === item);
    } else {
      return elements.find((e: Element) => e.number === item);
    }
  }).filter(Boolean) as Element[];
});

const getSymbolName = () => orderedElements.value.map((e: Element) => e.symbol).join('');

const exportPNG = async () => {
  if (tableRef.value) {
    const canvas = await html2canvas(tableRef.value, {
      scale: 10, // 控制清晰度
    });
    const link = document.createElement('a');
    link.href = canvas.toDataURL('image/png');
    link.download = getSymbolName() + '.png';
    link.click();
  }
};
</script>

<style scoped>
.element-table {
  display: inline-flex;
  flex-wrap: nowrap;
  background: #f8f9fa;
  padding: 10px;
  border-radius: 12px;
  box-shadow: 0 2px 8px #e0e0e0;
  justify-content: center;
  align-items: center;
  gap: 5px;
}
button {
  padding: 8px 16px;
  background: #1976d2;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background: #1565c0;
}
</style> 