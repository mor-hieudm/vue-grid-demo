<template>
  <div class="grid">
    <grid-layout :layout.sync="getLayout" :col-num="totalCol + blankGridItem.w" :row-height="30"
      :is-resizable="resizable" :responsive="false" :vertical-compact="false" :prevent-collision="true"
      :use-css-transforms="true" :margin="[0, 0]" :style="{ '--width': getContainerWidth }">

      <grid-item :static="true" :x="0" :y="0" :w="blankGridItem.w" :h="blankGridItem.h" :i="-1">
        <span class="text"></span>
      </grid-item>

      <grid-item :static="true" :x="blankGridItem.w" :y="0" :w="totalCol" :h="1" :i="-2">
        <span class="text">バリューチェーン</span>
      </grid-item>

      <grid-item :static="true" :x="0" :y="blankGridItem.h" :w="2" :h="getTotalRow" :i="-2">
        <span class="text">バリューチェーン</span>
      </grid-item>

      <grid-item v-for="item in columnHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :maxH="2" :minH="2" :i="item.i" :isDraggable="item.isDraggable" class="column-header"
        :style="{ '--borderHeight': getGridBorderHeight }" @resize="columnHeaderResizeEvent">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>

      <grid-item v-for="item in rowHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :minW="2" :maxW="2" :i="item.i" :isDraggable="item.isDraggable" @resize="rowHeaderResizeEvent"
        :style="{ '--borderWidth': getGridBorderWidth }" class="row-header">
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
      totalRow: 0,
      blankGridItem: { w: 4, h: 3 },
      columnHeader: [
        { id: 1, static: false, element: { label: '事業企画・営業企画' }, isDraggable: false },
        { id: 2, static: false, element: { label: '事業企画・営業企画' }, isDraggable: false },
        { id: 3, static: false, element: { label: 'フロント営業' }, isDraggable: false },
        { id: 4, static: false, element: { label: '導入コンサル・プリセールス' }, isDraggable: false },
        { id: 5, static: false, element: { label: 'PM' }, isDraggable: false },
        { id: 6, static: false, element: { label: '構築・プロビ' }, isDraggable: false },
      ],
      rowHeader: [
        { id: 7, static: false, element: { label: '探索領域' }, isDraggable: false },
        { id: 8, static: false, element: { label: 'モビリティ' }, isDraggable: false },
        { id: 9, static: false, element: { label: '物流' }, isDraggable: false },
      ],
      data: [
        { "x": 6, "y": 6, "w": 10, "h": 2, "i": "10", static: false, element: { label: 'Hehe' }, isDraggable: true, column: [], row: [] },
      ],
      draggable: true,
      resizable: true,
      index: 0,
      refreshKey: true,
    };
  },
  created() {
    const headerColW = (this.totalCol - this.totalCol % this.columnHeader.length) / this.columnHeader.length;
    const headerColX = (index) => this.blankGridItem.w + headerColW * index
    const _columnHeader = this.columnHeader.map((item, index) => ({ ...item, 'i': item.id, 'x': headerColX(index), 'y': 1, 'w': headerColW, 'h': 2 }))
    _columnHeader[this.columnHeader.length - 1].w += this.totalCol % this.columnHeader.length;
    this.columnHeader = _columnHeader

    const headerRowH = 10
    const headerRowy = (index) => this.blankGridItem.h + headerRowH * index
    const _rowHeader = this.rowHeader.map((item, index) => ({ ...item, 'i': item.id, 'x': 2, 'y': headerRowy(index), 'w': 2, 'h': headerRowH }))
    this.rowHeader = _rowHeader
  },
  computed: {
    getLayout() {
      return [...this.columnHeader, ...this.rowHeader, ...this.data]
    },
    getTotalRow() {
      return this.rowHeader.reduce(
        (accumulator, currentValue) => {
          return (accumulator + currentValue.h)
        },
        0
      );
    },
    getTotalCol() {
      return this.columnHeader.reduce(
        (accumulator, currentValue) => {
          return (accumulator + currentValue.w)
        },
        0
      );
    },
    getGridBorderWidth() {
      return (this.totalCol * 30 - 60) + 'px'
    },
    getGridBorderHeight() {
      return (this.getTotalRow * 30 + 60) + 'px'
    },
    getContainerWidth() {
      return this.totalCol * 30 + 'px'
    }
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
      if (resizeElement && this.totalCol <= this.getTotalCol) {
        this.totalCol -= amountOfGridChange
      }
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
  width: var(--width);
}

.vue-grid-item:not(.vue-grid-placeholder) {
  background: #ccc;
  border: 1px solid #DCDCDC;
}

.vue-grid-item .resizing {
  opacity: 0.9;
}

.column-header::after {
  display: inline-block;
  content: "";
  border-left: 1px solid #DCDCDC;
  height: var(--borderHeight);
  position: absolute;
  top: 0px;
  right: 0px;
}

.row-header::after {
  display: inline-block;
  content: "";
  border-bottom: 1px solid #DCDCDC;
  width: var(--borderWidth);
  position: absolute;
  left: 0px;
  bottom: 0px;
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
