<img src="imgs/FUCK.png" alt="logo" width="40%" height="40%" align="right" />

# ChemElemSplice

化学元素拼接器

## Usage

```
npm install
npm run dev
```

```ts
// src/elements.ts
export const customOrder: number[] = [9,92,6,19]; // FUCK
```

```js
const exportPNG = async () => {
  if (tableRef.value) {
    const canvas = await html2canvas(tableRef.value, {
      scale: 2, // 控制清晰度
    });
    const link = document.createElement('a');
    link.href = canvas.toDataURL('image/png');
    link.download = getSymbolName() + '.png';
    link.click();
  }
};
```

## Examples

| ![mtf](imgs/MtF.png) | ![pass](imgs/PAsS.png) |
|---|---|
| ![氢砹锝](imgs/HAtTc.png) | ![baby](imgs/BaBY.png) |
| ![bilibili](imgs/BiLiBiLi.png) | ![nb](imgs/NB.png) |
| ![nice](imgs/NiCe.png) | ![omg](imgs/OMg.png) |
