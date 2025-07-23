<template>
  <div class="element-block">
    <div class="top-row">
      <div class="number">{{ props.element.number }}</div>
      <div class="mass">
        <span v-if="props.element.mass !== null">{{ props.element.mass }}</span>
      </div>
    </div>
    <div class="symbol">{{ props.element.symbol }}</div>
    <div class="name">{{ props.element.name }}</div>
  </div>
</template>

<script lang="ts" setup>
import type { Element } from '../elements';

const props = defineProps<{ element: Element }>();
</script>

<style scoped>
.element-block {
  width: 80px;
  height: 100px;
  border: 2px solid #1976d2;
  border-radius: 8px;
  margin: 0px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #fff;
  box-shadow: 2px 2px 8px #e0e0e0;
  padding: 4px;
  box-sizing: border-box;
}
.top-row {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2px;
  padding: 0 0px;
}
.number {
  font-size: 14px;
  color: #1976d2;
}
.mass {
  font-size: 10px;
  color: #888;
}
.symbol {
  font-size: 32px;
  font-weight: bold;
  color: #1976d2;
}
.name {
  font-size: 10px;
  color: #333;
}
</style> 