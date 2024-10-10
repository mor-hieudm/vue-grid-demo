<template>
  <div class="g">
    <grid-layout :layout.sync="layout" :col-num="12" :row-height="30" :is-draggable="draggable"
      :is-resizable="resizable" :responsive="false" :vertical-compact="false" :prevent-collision="true"
      :use-css-transforms="true">
      <grid-item v-for="item in layout" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :i="item.i">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>
    </grid-layout>
  </div>
</template>
<script>
import { GridLayout, GridItem } from "vue-grid-layout"
export default {
  name: "GridBroad",
  props: {
    msg: String,
  },
  components: {
    GridLayout,
    GridItem
  },
  data() {
    return {
      count: 0,
      layout: [
        { "x": 1, "y": 0, "w": 2, "h": 2, "i": "0", static: false, element: { label: '事業企画・営業企画' } },
        { "x": 3, "y": 0, "w": 2, "h": 2, "i": "1", static: false, element: { label: 'フロント営業' } },
        { "x": 5, "y": 0, "w": 2, "h": 2, "i": "2", static: false, element: { label: '導入コンサル・プリセールス' } },
        { "x": 7, "y": 0, "w": 2, "h": 2, "i": "3", static: false, element: { label: 'PM' } },
        { "x": 9, "y": 0, "w": 2, "h": 2, "i": "4", static: false, element: { label: '構築・プロビ' } },
        { "x": 0, "y": 1, "w": 2, "h": 2, "i": "5", static: false, element: { label: '探索領域' } },
        { "x": 0, "y": 3, "w": 1, "h": 2, "i": "6", static: false, element: { label: 'モビリティ' } },
        { "x": 0, "y": 5, "w": 1, "h": 2, "i": "7", static: false, element: { label: '物流' } },
      ],
      horizontalHeader: [
        { id: 1, label: '事業企画・営業企画', position: { "x": 0, "y": 0, "w": 2, "h": 2, "i": "0", static: true } },
        { id: 2, label: 'フロント営業', position: { "x": 0, "y": 0, "w": 2, "h": 2, "i": "0", static: true } },
        { id: 3, label: "導入コンサル・プリセールス", position: { "x": 0, "y": 0, "w": 2, "h": 2, "i": "0", static: true } },
        { id: 4, label: 'PM', position: { "x": 0, "y": 0, "w": 2, "h": 2, "i": "0", static: true } },
        { id: 5, lable: '構築・プロビ', position: { "x": 0, "y": 0, "w": 2, "h": 2, "i": "0", static: true } }
      ],
      verticalHeader: [
        { id: 1, label: '探索領域' },
        { id: 2, label: 'モビリティ' },
        { id: 3, label: '物流' }
      ],
      draggable: true,
      resizable: true,
      index: 0
    };
  },
  methods: {
    itemTitle(item) {
      let result = item.i;
      if (item.static) {
        result += " - Static";
      }
      return result;
    }
  }
};
</script>

<style scoped>
.vue-grid-layout {
  background: #eee;
}

.vue-grid-item:not(.vue-grid-placeholder) {
  background: #ccc;
  border: 1px solid black;
}

.vue-grid-item .resizing {
  opacity: 0.9;
}

.vue-grid-item .static {
  background: #cce;
}

.vue-grid-item .text {
  font-size: 16px;
  display: flex;
  margin: 4px;
}

.vue-grid-item .no-drag {
  height: 100%;
  width: 100%;
}

.vue-grid-item .minMax {
  font-size: 12px;
}

.vue-grid-item .add {
  cursor: pointer;
}

.vue-draggable-handle {
  position: absolute;
  width: 20px;
  height: 20px;
  top: 0;
  left: 0;
  background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='10'><circle cx='5' cy='5' r='5' fill='#999999'/></svg>") no-repeat;
  background-position: bottom right;
  padding: 0 8px 8px 0;
  background-repeat: no-repeat;
  background-origin: content-box;
  box-sizing: border-box;
  cursor: pointer;
}
</style>