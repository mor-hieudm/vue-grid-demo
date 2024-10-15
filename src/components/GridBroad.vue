<template>
  <div class="grid-container">
    <grid-layout :layout.sync="getLayout" :col-num="totalCol + blankGridItem.w + 2" :row-height="30"
      :is-resizable="resizable" :responsive="false" :vertical-compact="false" :prevent-collision="true"
      :use-css-transforms="true" :margin="[0, 0]" :style="{ '--width': getContainerWidth }">

      <grid-item :static="true" :x="0" :y="0" :w="blankGridItem.w" :h="blankGridItem.h" :i="-1">
        <div class="grid-blank">
          <div></div>
          <div>横軸</div>
          <div>縦軸</div>
          <div></div>
        </div>
      </grid-item>

      <grid-item :static="true" :x="blankGridItem.w" :y="0" :w="totalCol" :h="1" :i="-2">
        <span>バリューチェーン</span>
      </grid-item>

      <grid-item :static="true" :x="0" :y="blankGridItem.h" :w="1" :h="getTotalRow" :i="-2">
        <span>バリューチェーン</span>
      </grid-item>

      <grid-item v-for="item in columnHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :maxH="2" :minH="2" :i="item.i" :isDraggable="item.isDraggable" class="column-header"
        :style="{ '--borderHeight': getGridBorderHeight }" @resize="columnHeaderResizeEvent">
        <span class="text">{{ item.element.label }}</span>
      </grid-item>

      <grid-item v-for="item in rowHeader" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :minW="2" :maxW="2" :i="item.i" :isDraggable="item.isDraggable" @resize="rowHeaderResizeEvent"
        :style="{ '--borderWidth': getGridBorderWidth }" class="row-header">
        <div class="row-header-text">{{ item.element.label }}</div>
      </grid-item>

      <grid-item v-for="item in data" :key="item.i" :static="item.static" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :i="item.i"
        :isDraggable="item.isDraggable" class="card-container" @resized="dataResizeEvent" @moved="dataResizeEvent">
        <div class="card-header">
          <div class="header-title">{{ item.element.label }}</div>
          <EditIcon />
        </div>
        <div class="card-body">
          <div v-for="item2 in item.element.score" :key="item.id" class="body-item">
            <div class="mark-context">{{ item2.mark }}</div>
            <div class="mark-sub-context">
              <div>{{ item2.number }}名</div>
              <div>({{ item2.percent }})</div>
            </div>
          </div>


        </div>
        <div class="card-footer">
          <div>{{ item.element.totalNum }}名</div>
          <div class="footer-sub-title">（20.0%）</div>
        </div>
      </grid-item>
    </grid-layout>
  </div>
</template>
<script>
import { GridLayout, GridItem } from "vue-grid-layout"
import EditIcon from "../assets/icon/EditIcon.vue"
export default {
  name: "GridBroad",
  props: {
    msg: String,
  },
  components: {
    GridLayout,
    GridItem, EditIcon
  },
  data() {
    return {
      totalCol: 100,
      totalRow: 0,
      blankGridItem: { w: 3, h: 3 },
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
        {
          "x": 6, "y": 16, "w": 10, "h": 10, "i": "11", static: false, element: {
            label: '事業企画・事業管理',
            score: [
              { id: 1, mark: 'A', number: 4, percent: "5%" },
              { id: 2, mark: 'B+', number: 5, percent: "6,26%" },
              { id: 3, mark: 'B', number: 24, percent: "30%" },
              { id: 4, mark: 'C+', number: 20, percent: "25%" },
              { id: 5, mark: 'C', number: 15, percent: "18,75%" },
              { id: 6, mark: 'D+', number: 4, percent: "5%" },
            ],
            totalNum: 80
          }, isDraggable: true, column: [], row: []
        },
        {
          "x": 6, "y": 6, "w": 10, "h": 10, "i": "12", static: false, element: {
            label: 'テクノロジスト',
            score: [
              { id: 1, mark: 'A', number: 4, percent: "5%" },
              { id: 2, mark: 'B+', number: 5, percent: "6,26%" },
              { id: 3, mark: 'B', number: 24, percent: "30%" },
              { id: 4, mark: 'C+', number: 20, percent: "25%" },
              { id: 5, mark: 'C', number: 15, percent: "18,75%" },
              { id: 6, mark: 'D+', number: 4, percent: "5%" },
            ],
            totalNum: 80
          }, isDraggable: true, column: [], row: []
        },
        {
          "x": 22, "y": 6, "w": 10, "h": 10, "i": "10", static: false, element: {
            label: '事業企画・事業管理',
            score: [
              { id: 1, mark: 'A', number: 4, percent: "5%" },
              { id: 2, mark: 'B+', number: 5, percent: "6,26%" },
              { id: 3, mark: 'B', number: 24, percent: "30%" },
              { id: 4, mark: 'C+', number: 20, percent: "25%" },
              { id: 5, mark: 'C', number: 15, percent: "18,75%" },
              { id: 6, mark: 'D+', number: 4, percent: "5%" },
            ],
            totalNum: 80
          }, isDraggable: true, column: [], row: []
        },
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
    const _rowHeader = this.rowHeader.map((item, index) => ({ ...item, 'i': item.id, 'x': 1, 'y': headerRowy(index), 'w': 2, 'h': headerRowH }))
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
      return (this.totalCol * 30 - 88) + 'px'
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
    findClosestNumber(array, x) {
      return array.filter(num => num <= x).reduce((prev, curr) => Math.abs(curr - x) < Math.abs(prev - x) ? curr : prev, -Infinity);
    },
    findColumnRange(currentData) {
      let fromX = this.findClosestNumber(this.columnHeader.map(data => data.x), currentData.x)
      let fromXColumn = this.columnHeader.find(data => data.x === fromX)
      let toX = currentData.x + currentData.w > fromXColumn.x + fromXColumn.w ? this.findClosestNumber(this.columnHeader.map(data => data.x + data.w), currentData.x + currentData.w) : 0
      if(toX !== 0) {
        let toXColumn = this.columnHeader.find(data => data.x === toX)
        return currentData.x + currentData.w === toXColumn.x ? this.columnHeader.filter(data => data.x >= fromX && data.x <= toX).map(data => data.i).filter(data => data !== toXColumn.i) : this.columnHeader.filter(data => data.x >= fromX && data.x <= toX).map(data => data.i)
      }
      return [fromXColumn.i]
    },
    findRowRange(currentData) {
      let fromY = this.findClosestNumber(this.rowHeader.map(data => data.y), currentData.y)
      let fromYColumn = this.rowHeader.find(data => data.y === fromY)
      let toY = currentData.y + currentData.h > fromYColumn.y + fromYColumn.h ? this.findClosestNumber(this.rowHeader.map(data => data.y + data.h), currentData.y + currentData.h) : 0
      if(toY !== 0) {
        let toYColumn = this.rowHeader.find(data => data.y === toY)
        return currentData.y + currentData.h === toYColumn.y ? this.rowHeader.filter(data => data.y >= fromY && data.y <= toY).map(data => data.i).filter(data => data !== toYColumn.i) : this.rowHeader.filter(data => data.y >= fromY && data.y <= toY).map(data => data.i)
      }
      return [fromYColumn.i]
    },
    dataResizeEvent: function (i) {
      let currentData = this.data.find(data => data.i === i)
      currentData.column = this.findColumnRange(currentData)
      currentData.row = this.findRowRange(currentData)
    }
  }
};
</script>

<style scoped>
.grid-blank {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  height: 100%;
  align-items: center;
}

.vue-grid-item:not(.vue-grid-placeholder) > .vue-resizable-handle {
  cursor: col-resize !important;
}

.card-header {
  background-color: #007BC3;
  color: #FFFFFF;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
  padding: 12.5px 8px;
  font-size: 12px;
}

.body-item {
  display: flex;
  flex-direction: row;
  padding: 8px 16px;
  justify-content: space-between;
}

.card-body {
  flex: 1;
  border-left: 1px solid #DCDCDC;
  border-right: 1px solid #DCDCDC;
}

.mark-sub-context {
  display: flex;
  width: 83px;
  justify-content: space-between;
  font-size: 10px;
  color: #666666;

}

.card-footer {
  display: flex;
  justify-content: flex-end;
  align-items: baseline;
  padding-right: 16px;
  padding-bottom: 5px;
  padding-top: 8px;
  border: 1px solid #DCDCDC;
}


.footer-sub-title {
  font-size: 10px;
}

.header-title {}

.card-container {
  border: none !important;
  background-color: #F8F8F8 !important;
  display: flex;
  flex-direction: column;
  border-radius: 6px;
}

.grid-blank ::after {
  display: inline-block;
  content: "";
  border-bottom: 1px solid #DCDCDC;
  width: 125px;
  position: absolute;
  left: -20px;
  bottom: 44px;
  z-index: 99;
  transform: rotate(45deg);
}

.vue-grid-layout {
  width: var(--width);
}

.vue-grid-item:not(.vue-grid-placeholder) {
  background: #EDEDED;
  border: 1px solid #DCDCDC;
  align-content: center;
}
.card-container {
  touch-action: none;
  box-sizing: border-box;
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

.column-header {
  background-color: #F8F8F8 !important;
  border-top: none !important;
}

.row-header {
  background-color: #F8F8F8 !important;

}

.row-header-text {
  word-break: break-all;
  padding: 0px 13px;
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
/deep/.column-header .vue-resizable-handle {
  background: none !important;
  cursor: col-resize !important;

}
</style>
