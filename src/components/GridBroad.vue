<template>
  <div class="grid">
    <grid-layout :layout.sync="getLayout" :col-num="12" :row-height="30"
      :is-resizable="resizable" :responsive="false" :vertical-compact="false" :prevent-collision="true"
      :use-css-transforms="true" :margin="[0,0]">
      <grid-item v-for="item in columnHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :i="item.i" :isDraggable="item.isDraggable" class="column">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>
      <grid-item v-for="item in rowHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
                 :i="item.i" :isDraggable="item.isDraggable" class="row">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>
      <grid-item v-for="item in data" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
                 :i="item.i" :isDraggable="item.isDraggable" class="data">
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
      columnHeader: [
        { "x": 1, "y": 0, "w": 2, "h": 2, "i": "0", static: false, element: { label: '事業企画・営業企画' }, isDraggable: false },
        { "x": 3, "y": 0, "w": 2, "h": 2, "i": "1", static: false, element: { label: 'フロント営業' }, isDraggable: false },
        { "x": 5, "y": 0, "w": 2, "h": 2, "i": "2", static: false, element: { label: '導入コンサル・プリセールス' }, isDraggable: false },
        { "x": 7, "y": 0, "w": 2, "h": 2, "i": "3", static: false, element: { label: 'PM' }, isDraggable: false },
        { "x": 9, "y": 0, "w": 2, "h": 2, "i": "4", static: false, element: { label: '構築・プロビ' }, isDraggable: false },
      ],
      rowHeader: [
        { "x": 0, "y": 2, "w": 1, "h": 2, "i": "5", static: false, element: { label: '探索領域' }, isDraggable: false },
        { "x": 0, "y": 4, "w": 1, "h": 2, "i": "6", static: false, element: { label: 'モビリティ' }, isDraggable: false },
        { "x": 0, "y": 6, "w": 1, "h": 2, "i": "7", static: false, element: { label: '物流' }, isDraggable: false },
        { "x": 0, "y": 8, "w": 1, "h": 2, "i": "8", static: false, element: { label: '物流' }, isDraggable: false },
        { "x": 0, "y": 10, "w": 1, "h": 2, "i": "9", static: false, element: { label: '物流' }, isDraggable: false },
      ],
      data: [
        { "x": 3, "y": 6, "w": 1, "h": 2, "i": "10", static: false, element: { label: 'Hehe' },isDraggable: true, column: [], row: [] },
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
  watch: {
    data: {

    },
    columnHeader: {
      handler() {
        let firstWidth = this.columnHeader[0].w
        let firstX = this.columnHeader[0].x
        this.columnHeader = this.columnHeader.map((data, index) => {
          if(index > 0) {
            data.x = firstX + firstWidth
            firstX = data.w
            firstWidth = data.x
          }
          return data
        })
      },
      deep: true,
    },
  },
  computed: {
    getMaxWidth() {
      return this.columnHeader.map(data => data.w).reduce(
          (accumulator, currentValue) => accumulator + currentValue,
          1,
      );
    },
    getLayout() {
      return [...this.columnHeader,...this.rowHeader,...this.data]
    }
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
.grid::before {
  content: '';
  background-size: calc(calc(100% - 5px) / 12) 40px;
  background-image: linear-gradient(
      to right,
      lightgrey 1px,
      transparent 1px
  ),
  linear-gradient(to bottom, lightgrey 1px, transparent 1px);
  height: calc(100% - 5px);
  width: calc(100% - 5px);
  /*height: 100%;*/
  /*width: 100%;*/
  position: absolute;
  background-repeat: repeat;
  margin:5px;
}
.column::before {
  content: '';
  position: absolute;
  border-bottom: 1px solid black;
  right: 0;
}

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
  font-size: 24px;
  text-align: center;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  height: 100%;
  width: 100%;
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
