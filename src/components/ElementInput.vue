<template>
  <div class="input-section">
    <p class="input-description">输入字母，看看它是否能用化学元素符号"拼写"出来</p>
    <input 
      id="customOrder"
      v-model="customOrderInput" 
      type="text" 
      placeholder="例如:Pregabalin"
      @input="updateCustomOrder"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref, computed, watch } from 'vue';
import elementsData from '../assets/elements.json';
import type { Element } from '../types/element';

const elements: Element[] = elementsData.elements;

const props = defineProps<{
  modelValue: (number | string)[]
}>();

const emit = defineEmits<{
  'update:modelValue': [value: (number | string)[]],
  'update:rawInput': [value: string]
}>();

const customOrderInput = ref('');

const elementData: Record<string, Element> = {};
elements.forEach(element => {
  elementData[element.symbol.toLowerCase()] = element;
});

const memo = new Map<string, (number | string)[] | null>();

const findElementalSpelling = (word: string): (number | string)[] | null => {
  if (memo.has(word)) {
    return memo.get(word) || null;
  }
  
  if (word === '') {
    return [];
  }

  const oneLetterSymbol = word.substring(0, 1);
  if (elementData[oneLetterSymbol]) {
    const restOfWord = word.substring(1);
    const result = findElementalSpelling(restOfWord);
    if (result !== null) {
      const finalResult = [elementData[oneLetterSymbol].symbol, ...result];
      memo.set(word, finalResult);
      return finalResult;
    }
  }

  if (word.length >= 2) {
    const twoLetterSymbol = word.substring(0, 2);
    if (elementData[twoLetterSymbol]) {
      const restOfWord = word.substring(2);
      const result = findElementalSpelling(restOfWord);
      if (result !== null) {
        const finalResult = [elementData[twoLetterSymbol].symbol, ...result];
        memo.set(word, finalResult);
        return finalResult;
      }
    }
  }

  memo.set(word, null);
  return null;
};

const parseCustomOrder = (input: string): (number | string)[] => {
  const cleanInput = input.toLowerCase().replace(/[^a-z]/g, '');
  
  const spelling = findElementalSpelling(cleanInput);
  if (spelling) {
    return spelling;
  }
  
  return [];
};

const customOrder = computed(() => parseCustomOrder(customOrderInput.value));

const orderedElements = computed(() => {
  return customOrder.value.map((item: number | string) => {
    if (typeof item === 'string') {
      return elements.find((e: Element) => e.symbol.toUpperCase() === item.toUpperCase());
    } else {
      return elements.find((e: Element) => e.number === item);
    }
  }).filter(Boolean) as Element[];
});

const getSymbolName = () => orderedElements.value.map((e: Element) => e.symbol).join('');

const updateCustomOrder = () => {
  emit('update:modelValue', customOrder.value);
  emit('update:rawInput', customOrderInput.value);
};

watch(() => props.modelValue, (newValue: (number | string)[]) => {
  if (newValue && newValue.length > 0) {
    customOrderInput.value = newValue.join('');
  }
}, { immediate: true });

updateCustomOrder();
</script>

<style scoped>
.input-section {
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.input-description {
  margin-bottom: 10px;
  text-align: center;
  color: #fff;
  font-size: 16px;
  word-break: break-all;
  max-width: 100vw;
  white-space: normal;
}

.input-section input {
  padding: 6px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  min-width: 200px;
}

.input-section input:focus {
  outline: none;
  border-color: #1976d2;
  box-shadow: 0 0 0 2px rgba(25, 118, 210, 0.2);
}
</style> 