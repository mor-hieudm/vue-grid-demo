<template>
  <div class="grid-container">
    <grid-layout :layout.sync="getLayout" :col-num="totalCol + blankGridItem.w + 2" :row-height="30"
      :is-resizable="true" :responsive="false" :vertical-compact="false" :prevent-collision="true"
      :use-css-transforms="true" :margin="[0, 0]" :style="{ '--width': getContainerWidth }">

      <grid-item :static="true" :x="0" :y="0" :w="blankGridItem.w" :h="blankGridItem.h" :i="-1">
        <div class="grid-blank">
          <div></div>
          <div>横軸</div>
          <div>縦軸</div>
          <div></div>
        </div>
      </grid-item>

      <grid-item class="header-main-title" :static="true" :x="blankGridItem.w" :y="0" :w="totalCol" :h="1" :i="-2">
        <span>バリューチェーン</span>
      </grid-item>

      <grid-item class="header-main-title" :static="true" :x="0" :y="blankGridItem.h" :w="1" :h="getTotalRow" :i="-2">
        <span>バリューチェーン</span>
      </grid-item>

      <grid-item class="column-header" v-for="item in columnHeader" :x="item.x" :y="item.y" :w="item.w" :h="item.h"
        :maxH="2" :minH="2" :i="item.i" :isResizable="true" :isDraggable="false"
        :style="{ '--borderHeight': getGridBorderHeight }" @resize="columnHeaderResizeEvent" @resized="calculateData">
        <div>{{ item.element.label }}</div>
      </grid-item>

      <grid-item class="row-header" v-for="item in rowHeader" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :minW="2"
        :maxW="2" :i="item.i" :isResizable="true" :isDraggable="false" @resize="rowHeaderResizeEvent" @resized="calculateData"
        :style="{ '--borderWidth': getGridBorderWidth }">
        <div class="row-header-text">{{ item.element.label }}</div>
      </grid-item>

      <grid-item v-for="item in data" :key="item.i" :static="item.static" :x="item.x" :y="item.y" :w="item.w"
        :h="item.h" :i="item.i" :isDraggable="item.isDraggable" class="card-container" @resized="dataResizeEvent"
        @moved="dataResizeEvent" :minW="6" :minH="10">
        <GridCard :label="item.element.label" :score="item.element.score" :totalNum="item.element.totalNum" />
      </grid-item>
    </grid-layout>
  </div>
</template>
<script>
import { GridLayout, GridItem } from "vue-grid-layout"
import EditIcon from "../assets/icon/EditIcon.vue"
import GridCard from "./GridCard.vue";
export default {
  name: "GridBroad",
  components: {
    GridLayout,
    GridItem, EditIcon, GridCard
  },
  data() {
    return {
      totalCol: 100,
      totalRow: 0,
      gridPadding: 1,
      blankGridItem: { w: 3, h: 3 },
      defaultGridItem: { w: 6, h: 10 },
      parentColHeader: { w: this.totalCol, h: 1 },
      parentRowHeader: { w: 1, h: this.getTotalRow },
      columnHeader: [
        { id: 1, element: { label: '事業企画・営業企画', gridChildIds: [] } },
        { id: 2, element: { label: '事業企画・営業企画', gridChildIds: [] } },
        { id: 3, element: { label: 'フロント営業', gridChildIds: [] } },
        { id: 4, element: { label: '導入コンサル・プリセールス', gridChildIds: [] } },
        { id: 5, element: { label: 'PM', gridChildIds: [] } },
        { id: 6, element: { label: '構築・プロビ', gridChildIds: [] } },
      ],

      rowHeader: [
        { id: 7, element: { label: '探索領域', gridChildIds: [] }, },
        { id: 8, element: { label: 'モビリティ', gridChildIds: [] }, },
        { id: 9, element: { label: '物流', gridChildIds: [] }, },
      ],

      data: [],

      mockApiData: [
        {
          id: 100,
          element: {
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
          }, column: [2], row: [7]
        },
        {
          id: 11,
          element: {
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
          }, column: [2], row: [7]
        },
        {
          id: 12,
          element: {
            label: 'テクノロジスト2',
            score: [
              { id: 1, mark: 'A', number: 4, percent: "5%" },
              { id: 2, mark: 'B+', number: 5, percent: "6,26%" },
              { id: 3, mark: 'B', number: 24, percent: "30%" },
              { id: 4, mark: 'C+', number: 20, percent: "25%" },
              { id: 5, mark: 'C', number: 15, percent: "18,75%" },
              { id: 6, mark: 'D+', number: 4, percent: "5%" },
            ],
            totalNum: 80
          }, column: [4], row: [9]
        },
        {
          id: 13,
          element: {
            label: 'ソリューションSE',
            score: [
              { id: 1, mark: 'A', number: 4, percent: "5%" },
              { id: 2, mark: 'B+', number: 5, percent: "6,26%" },
              { id: 3, mark: 'B', number: 24, percent: "30%" },
              { id: 4, mark: 'C+', number: 20, percent: "25%" },
              { id: 5, mark: 'C', number: 15, percent: "18,75%" },
              { id: 6, mark: 'D+', number: 4, percent: "5%" },
            ],
            totalNum: 80
          }, column: [1], row: [8]
        },
      ],
    };
  },
  created() {
    //calculate ColHeader Grid
    const colYPosition = 1;
    let colXPosition = this.blankGridItem.w;
    this.columnHeader = this.columnHeader.map((col) => {
      let totalColWidth = 0;
      this.mockApiData.forEach((data) => {
        if (data.column.includes(col.id)) {
          totalColWidth += this.gridPadding + this.defaultGridItem.w
        }
      })

      if (!totalColWidth) {
        totalColWidth = this.gridPadding + this.defaultGridItem.w
      }

      const tempColHeader = {
        x: colXPosition,
        y: colYPosition,
        w: totalColWidth + this.gridPadding,
        h: this.blankGridItem.h - this.parentColHeader.h,
        i: col.id,
        ...col
      }
      colXPosition += tempColHeader.w
      return tempColHeader
    })
    this.totalCol = this.getTotalCol


    //calculate RowHeader Grid

    const rowXPosition = 1;
    let rowYPosition = this.blankGridItem.h;
    this.rowHeader = this.rowHeader.map((row) => {
      let totalColHeight = 0;

      if (!totalColHeight) {
        totalColHeight = this.gridPadding + this.defaultGridItem.h
      }

      const tempRowHeader = {
        x: rowXPosition,
        y: rowYPosition,
        w: this.blankGridItem.w - this.parentRowHeader.w,
        h: totalColHeight + this.gridPadding,
        i: row.id,
        ...row
      }
      rowYPosition += tempRowHeader.h
      return tempRowHeader
    })

    // calculate grid data
    const hashMapColStacks = {}
    for (const element of this.columnHeader) {
      hashMapColStacks[element.id] = [];
    }

    const hashMapRowStacks = {}
    for (const element of this.rowHeader) {
      hashMapRowStacks[element.id] = [];
    }
    this.data = this.mockApiData.map((data) => {
      let colXPosition = this.columnHeader.find((col) => col.id === data.column[0]).x + this.gridPadding
      if (hashMapColStacks[data.column[0]].length) {
        hashMapColStacks[data.column[0]].push(data.id)
        colXPosition += (this.defaultGridItem.w + this.gridPadding)
      } else {
        hashMapColStacks[data.column[0]].push(data.id)
      }

      let colYPosition = this.rowHeader.find((row) => row.id === data.row[0]).y + this.gridPadding
      hashMapRowStacks[data.row[0]].push(data.id)
      // if (hashMapRowStacks[data.row[0]].length) {
      //   hashMapRowStacks[data.row[0]].push(data.id)
      //   colYPosition += (this.defaultGridItem.h + gridPadding)
      // } else {
      //   hashMapRowStacks[data.row[0]].push(data.id)
      // }

      const tempData = {
        x: colXPosition,
        y: colYPosition,
        w: this.defaultGridItem.w,
        h: this.defaultGridItem.h,
        i: data.id,
        ...data
      }
      return tempData
    })

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
    calculateData() {
      this.data.map(data => {
        this.dataResizeEvent(data.i)
      })
    },
    findClosestNumber(array, x) {
      return array.filter(num => num <= x).reduce((prev, curr) => Math.abs(curr - x) < Math.abs(prev - x) ? curr : prev, -Infinity);
    },
    findColumnRange(currentData) {
      let fromX = this.findClosestNumber(this.columnHeader.map(data => data.x), currentData.x)
      let fromXColumn = this.columnHeader.find(data => data.x === fromX)
      let toX = currentData.x + currentData.w > fromXColumn.x + fromXColumn.w ? this.findClosestNumber(this.columnHeader.map(data => data.x + data.w), currentData.x + currentData.w) : 0
      if (toX !== 0) {
        let toXColumn = this.columnHeader.find(data => data.x === toX)
        return currentData.x + currentData.w === toXColumn.x ? this.columnHeader.filter(data => data.x >= fromX && data.x <= toX).map(data => data.i).filter(data => data !== toXColumn.i) : this.columnHeader.filter(data => data.x >= fromX && data.x <= toX).map(data => data.i)
      }
      return [fromXColumn.i]
    },
    findRowRange(currentData) {
      let fromY = this.findClosestNumber(this.rowHeader.map(data => data.y), currentData.y)
      let fromYColumn = this.rowHeader.find(data => data.y === fromY)
      let toY = currentData.y + currentData.h > fromYColumn.y + fromYColumn.h ? this.findClosestNumber(this.rowHeader.map(data => data.y + data.h), currentData.y + currentData.h) : 0
      if (toY !== 0) {
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

::v-deep.column-header .vue-resizable-handle {
  cursor: url(../assets/img/ColResize.svg) 4 12, auto;
}

::v-deep.row-header .vue-resizable-handle {
  cursor: url(../assets/img/RowResize.svg) 4 12, auto;
}

::v-deep .vue-resizable-handle {
  background: none !important;
}


::v-deep.column-header.resizing {
  cursor: url(../assets/img/ColResize.svg) 4 12, auto !important;
}

::v-deep.row-header.resizing {
  cursor: url(../assets/img/RowResize.svg) 4 12, auto !important;
}

.card-container {
  border: none !important;
  background-color: #F8F8F8 !important;
}

.header-main-title {
  display: flex;
  justify-content: center;
  align-items: center;
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
  display: flex;
  align-items: center;
}

.column-header div {
  padding-left: 8px;
}

.row-header {
  background-color: #F8F8F8 !important;
  display: flex;
  justify-content: center;
  align-items: center;
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
