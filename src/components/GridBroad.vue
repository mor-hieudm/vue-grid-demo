<template>
  <div>
    <grid-layout :layout.sync="horizontalHeader" :col-num="12" :row-height="30" :isDraggable="false" :margin="[0, 0]"
      :maxRows="3">
      <grid-item v-for="item in horizontalHeader" :isResizable="true" :static="item.static" :x="item.x" :y="item.y"
        :w="item.w" :h="item.h" :i="item.i" @resize="horizontalHeaderResizeEvent">
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
      horizontalHeader: [
        { "x": 0, "y": 0, "w": 1, "h": 3, "i": "-1", static: true, element: {} },
        { "x": 1, "y": 0, "w": 10, "h": 1, "i": "-2", static: true, element: { label: 'バリューチェーン' } },
        { "x": 1, "y": 1, "w": 2, "h": 2, "i": "0", static: false, element: { label: '事業企画・営業企画' } },
        { "x": 3, "y": 1, "w": 2, "h": 2, "i": "1", static: false, element: { label: 'フロント営業' } },
        { "x": 5, "y": 1, "w": 2, "h": 2, "i": "2", static: false, element: { label: '導入コンサル・プリセールス' } },
        { "x": 7, "y": 1, "w": 2, "h": 2, "i": "3", static: false, element: { label: 'PM' } },
        { "x": 9, "y": 1, "w": 2, "h": 2, "i": "4", static: false, element: { label: '構築・プロビ' } },

      ],
      verticalLayout: [
        { "x": 0, "y": 2, "w": 1, "h": 2, "i": "5", static: false, element: { label: '探索領域' } },
        { "x": 0, "y": 4, "w": 1, "h": 5, "i": "6", static: false, element: { label: 'モビリティ' } },
        { "x": 0, "y": 6, "w": 1, "h": 2, "i": "7", static: false, element: { label: '物流' } },
      ],
    };
  },
  methods: {
    horizontalHeaderResizeEvent: function (i, newH, newW, newHPx, newWPx) {
      let resizeElement = false
      let amountOfGridChange = 0;
      this.horizontalHeader.forEach((header, index) => {
        if (resizeElement) {
          header.x -= amountOfGridChange
        }
        if (header.i === i) {
          amountOfGridChange = header.w - newW
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