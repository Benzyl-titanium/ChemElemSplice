<img src="docs/imgs/FUCK.png" alt="logo" width="40%" height="40%" align="right" />

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
// src/components/ElementTable.vue
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
```

## Examples

| ![mtf](docs/imgs/MtF.png) | ![pass](docs/imgs/PAsS.png) |
|---|---|
| ![氢砹锝](docs/imgs/HAtTc.png) | ![baby](docs/imgs/BaBY.png) |
| ![bilibili](docs/imgs/BiLiBiLi.png) | ![nb](docs/imgs/NB.png) |
| ![nice](docs/imgs/NiCe.png) | ![omg](docs/imgs/OMg.png) |

- [more](docs/examples/imgs)