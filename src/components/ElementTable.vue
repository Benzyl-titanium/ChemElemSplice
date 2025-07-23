<template>
  <div>
    <div ref="tableRef" class="element-table">
      <ElementBlock
        v-for="(el, idx) in orderedElements"
        :key="el.number"
        :element="el"
        :isFirst="idx === 0"
        :isLast="idx === orderedElements.length - 1"
      />
    </div>
    <div style="margin-top: 16px;">
      <button @click="exportPNG">PNG</button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue';
import ElementBlock from './ElementBlock.vue';
import { elements, customOrder } from '../elements';
import html2canvas from 'html2canvas';
// @ts-ignore
import domtoimage from 'dom-to-image-more';

const tableRef = ref<HTMLElement | null>(null);

const orderedElements = computed(() => {
  return customOrder.map(num => elements.find(e => e.number === num)!).filter(Boolean);
});

const getSymbolName = () => orderedElements.value.map(e => e.symbol).join('');

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
  display: flex;
  flex-wrap: nowrap;
  overflow-x: auto;
  background: #f8f9fa;
  padding: 10px;
  border-radius: 12px;
  box-shadow: 0 2px 8px #e0e0e0;
  justify-content: center;
  align-items: center;
}
button {
  margin-right: 8px;
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