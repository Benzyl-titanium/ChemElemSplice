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
interface Element {
  symbol: string;
  name: string;
  number: number;
  mass: number | null;
}

const props = defineProps<{ element: Element }>();
</script>

<style scoped>
.element-block {
  width: 80px;
  height: 100px;
  border: 2px solid #1976d2;
  border-radius: 8px;
  margin: 0px;
  position: relative;
  background: #fff;
  box-shadow: 2px 2px 8px #e0e0e0;
  padding: 4px;
  box-sizing: border-box;
}
.top-row {
  position: absolute;
  top: 4px;
  left: 4px;
  right: 4px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.number {
  font-size: 14px;
  color: #1976d2;
}
.mass {
  font-size: 12px;
  color: #888;
}
.symbol {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 32px;
  font-weight: bold;
  color: #1976d2;
}
.name {
  position: absolute;
  bottom: 5px;
  left: 0px;
  right: 0px;
  font-size: 10px;
  color: #333;
  text-align: center;
}
</style> 