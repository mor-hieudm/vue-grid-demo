<template>
  <div class="grid">
    <grid-layout :layout.sync="getLayout" :col-num="totalCol + blankGridItem.w" :row-height="30"
      :is-resizable="resizable" :responsive="false" :vertical-compact="false" :prevent-collision="true"
      :use-css-transforms="true" :margin="[0, 0]">

      <grid-item :static="true" :x="0" :y="0" :w="blankGridItem.w" :h="blankGridItem.h" :i="-1" class="column-header">
        <span class="text"></span>
      </grid-item>

      <grid-item :static="true" :x="blankGridItem.w" :y="0" :w="totalCol" :h="1" :i="-2" class="column-header">
        <span class="text">バリューチェーン</span>
      </grid-item>

      <grid-item v-for="item in columnHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :maxH="2" :minH="2" :i="item.i" :isDraggable="item.isDraggable" class="column-header"
        @resize="columnHeaderResizeEvent">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>
      <grid-item v-for="item in rowHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :maxW="2" :i="item.i" :isDraggable="item.isDraggable" @resize="rowHeaderResizeEvent" class="row">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>
      <grid-item v-for="item in data" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :i="item.i"
        :isDraggable="item.isDraggable" class="data">
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
      totalCol: 100,
      blankGridItem: { w: 3, h: 3 },
      columnHeader: [
        { static: false, element: { label: '事業企画・営業企画' }, isDraggable: false },
        { static: false, element: { label: '事業企画・営業企画' }, isDraggable: false },
        { static: false, element: { label: 'フロント営業' }, isDraggable: false },
        { static: false, element: { label: '導入コンサル・プリセールス' }, isDraggable: false },
        { static: false, element: { label: 'PM' }, isDraggable: false },
        { static: false, element: { label: '構築・プロビ' }, isDraggable: false },
      ],
      rowHeader: [
        // {  static: false, element: { label: '探索領域' }, isDraggable: false },
        // {  static: false, element: { label: 'モビリティ' }, isDraggable: false },
        // {  static: false, element: { label: '物流' }, isDraggable: false },
        // {  static: false, element: { label: '物流' }, isDraggable: false },
        // {  static: false, element: { label: '物流' }, isDraggable: false },
      ],
      data: [
        { "x": 3, "y": 6, "w": 1, "h": 2, "i": "10", static: false, element: { label: 'Hehe' }, isDraggable: true, column: [], row: [] },
      ],
      draggable: true,
      resizable: true,
      index: 0,
      refreshKey: true,
    };
  },
  created() {
    const headerW = (this.totalCol - this.totalCol % this.columnHeader.length) / this.columnHeader.length;
    const headerX = (index) => this.blankGridItem.w + headerW * index
    const columnHeaderGrid = this.columnHeader.map((item, index) => ({ ...item, 'i': index, 'x': headerX(index), 'y': 1, 'w': headerW, 'h': 2 }))
    columnHeaderGrid[this.columnHeader.length - 1].w += this.totalCol % this.columnHeader.length;
    this.columnHeader = columnHeaderGrid
  },
  computed: {
    getMaxWidth() {
      return this.columnHeader.map(data => data.w).reduce(
        (accumulator, currentValue) => accumulator + currentValue,
        1,
      );
    },
    getLayout() {
      return [...this.columnHeader, ...this.rowHeader, ...this.data]
    },
  },
  methods: {
    columnHeaderResizeEvent: function (i, newH, newW) {
      let resizeElement = false
      let amountOfGridChange = 0;
      this.columnHeader.forEach((header) => {
        if (resizeElement) {
          header.x -= amountOfGridChange
        }
        if (header.i === i) {
          amountOfGridChange = header.w - newW
          resizeElement = true
        }
      });
    },
    rowHeaderResizeEvent: function (i, newH) {
      let resizeElement = false
      let amountOfGridChange = 0;
      this.rowHeader.forEach((header) => {
        if (resizeElement) {
          header.y -= amountOfGridChange
        }
        if (header.i === i) {
          amountOfGridChange = header.h - newH
          resizeElement = true
        }
      });
    },
  }
};
</script>

<style scoped>
.vue-grid-layout {
  background: #eee;
}

.vue-grid-item:not(.vue-grid-placeholder) {
  background: #ccc;
  border: 1px solid #DCDCDC;
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
