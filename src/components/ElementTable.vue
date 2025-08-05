<template>
  <div>
    <div v-if="orderedElements.length === 0 && props.rawInput && props.rawInput.trim() !== ''" style="color: #fff; text-align: center; margin: 24px 0; font-size: 18px;">
      无法用元素符号拼写 "{{ props.rawInput }}"
    </div>
    <div v-if="orderedElements.length > 0" class="element-table-wrapper">
      <div class="mode-switch">
        <button 
          @click="setMode('minimal')" 
          :class="{ active: mode === 'minimal' }"
          class="mode-btn"
        >
          短
        </button>
        <button 
          @click="setMode('maximal')" 
          :class="{ active: mode === 'maximal' }"
          class="mode-btn"
        >
          长
        </button>
      </div>
      <div ref="tableRef" class="element-table">
        <ElementBlock
          v-for="(el, idx) in orderedElements"
          :key="el.number"
          :element="el"
          :isFirst="idx === 0"
          :isLast="idx === orderedElements.length - 1"
        />
      </div>
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
import type { Element } from '../types/element';

const elements: Element[] = elementsData.elements;

const props = defineProps<{
  customOrder: (number | string)[],
  rawInput?: string
}>();

const tableRef = ref<HTMLElement | null>(null);
const mode = ref<'minimal' | 'maximal'>('minimal');

const findMinimalSpelling = (target: string): Element[] => {
  const targetUpper = target.toUpperCase();
  const n = targetUpper.length;
  
  const dp: number[] = new Array(n + 1).fill(Infinity);
  const prev: Element[] = new Array(n + 1);
  
  dp[0] = 0;
  
  for (let i = 0; i < n; i++) {
    if (dp[i] === Infinity) continue;
    
    for (const element of elements) {
      const symbol = element.symbol.toUpperCase();
      const symbolLen = symbol.length;
      
      if (i + symbolLen <= n && targetUpper.substring(i, i + symbolLen) === symbol) {
        if (dp[i + symbolLen] > dp[i] + 1) {
          dp[i + symbolLen] = dp[i] + 1;
          prev[i + symbolLen] = element;
        }
      }
    }
  }
  
  if (dp[n] === Infinity) {
    return [];
  }
  
  const result: Element[] = [];
  let pos = n;
  while (pos > 0) {
    result.unshift(prev[pos]);
    pos -= prev[pos].symbol.length;
  }
  
  return result;
};

const findMaximalSpelling = (target: string): Element[] => {
  const targetUpper = target.toUpperCase();
  const result: Element[] = [];
  let pos = 0;
  
  while (pos < targetUpper.length) {
    let found = false;
    
    for (const element of elements) {
      const symbol = element.symbol.toUpperCase();
      if (symbol.length === 1 && pos < targetUpper.length && targetUpper[pos] === symbol[0]) {
        result.push(element);
        pos += 1;
        found = true;
        break;
      }
    }
    
    if (!found) {
      for (const element of elements) {
        const symbol = element.symbol.toUpperCase();
        if (symbol.length === 2 && pos + 1 < targetUpper.length && 
            targetUpper[pos] === symbol[0] && targetUpper[pos + 1] === symbol[1]) {
          result.push(element);
          pos += 2;
          found = true;
          break;
        }
      }
    }
    
    if (!found) {
      return [];
    }
  }
  
  return result;
};

const orderedElements = computed(() => {
  if (props.rawInput && props.rawInput.trim() !== '') {
    if (mode.value === 'minimal') {
      return findMinimalSpelling(props.rawInput);
    } else {
      const maximalResult = findMaximalSpelling(props.rawInput);
      if (maximalResult.length === 0) {
        return findMinimalSpelling(props.rawInput);
      }
      return maximalResult;
    }
  } else {
    return props.customOrder.map((item: number | string) => {
      if (typeof item === 'string') {
        return elements.find((e: Element) => e.symbol === item);
      } else {
        return elements.find((e: Element) => e.number === item);
      }
    }).filter(Boolean) as Element[];
  }
});

const setMode = (newMode: 'minimal' | 'maximal') => {
  mode.value = newMode;
};

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
.mode-switch {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
  gap: 8px;
}

.mode-btn {
  padding: 8px 16px;
  background: #f0f0f0;
  color: #333;
  border: 2px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s ease;
}

.mode-btn:hover {
  background: #e0e0e0;
  border-color: #1976d2;
}

.mode-btn.active {
  background: #1976d2;
  color: #fff;
  border-color: #1976d2;
}

.element-table {
  display: inline-flex;
  flex-wrap: nowrap;
  background: #f8f9fa;
  padding: 10px;
  border-radius: 12px;
  box-shadow: 0 2px 8px #e0e0e0;
  justify-content: center;
  align-items: center;
  gap: 0px;
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