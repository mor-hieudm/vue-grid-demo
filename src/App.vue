<template>
  <div id="app">
    <div class="header-container">
      <div class="page-header">
        <div class="header-main-title">
          <HeightChart class="icon" />
          <div class="title-text">タイトルを入力してください</div>
          <Pencil />
        </div>
        <div class="header-button">
          <Dowload />
          <div>保存</div>
        </div>
      </div>

      <div class="page-sub-header">
        <div class="left-button-group">
          <div>機能名</div>
          <div class="status-as-is">As-Is</div>
          <div class="status-to-be">To-Be</div>
        </div>

        <div class="right-button-group">
          <div class="semi-button">評価の表示を設定する</div>
          <div class="active-button">+ 職種を追加する</div>
          <div class="dropdow-button">
            <div class="dropdow-title">Action</div>
            <CardetDown class="cardet-icon" />
          </div>
        </div>
      </div>
    </div>

    <GridBroad />
  </div>
</template>

<script>
import CardetDown from "./assets/icon/CardetDown.vue";
import Dowload from "./assets/icon/Dowload.vue";
import HeightChart from "./assets/icon/HeightChart.vue";
import Pencil from "./assets/icon/Pencil.vue";
import GridBroad from "./components/GridBroad.vue";
export default {
  components: {
    GridBroad,
    HeightChart,
    Pencil,
    Dowload,
    CardetDown
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  font-weight: 400;
  font-size: 16px;

}

body {
  margin: 0;
  width: max-content;
  display: inline-flex;
}

.header-container {
  position: sticky;
  background-color: #FFFFFF;
  z-index: 99;
  left: 0;
  width: 97vw;
}

.dropdow-button {
  display: flex;
  color: #FFFFFF;
  font-weight: 700;
  background-color: #007BC3;
  border-radius: 4px;
  align-items: center;
}

.cardet-icon {
  padding: 8px;
  border-left: 1px solid #FFFFFF;
}

.dropdow-title {
  padding: 4px 24px;
}

.semi-button {
  padding: 6px 12px;
  border: solid 1px #7BBBE0;
  color: #007BC3;
  border-radius: 4px;
  font-size: 14px;
}

.active-button {
  padding: 6px 12px;
  border: solid 1px #7BBBE0;
  color: #007BC3;
  border-radius: 4px;
  font-size: 14px;
  background-color: #E1F0F8;
}

.page-header {
  background-color: #285A89;
  color: #FFFFFF;
  padding: 8px 24px;
  width: 97vw;
  display: flex;
  justify-content: space-between;
}

.header-main-title {
  display: flex;
  align-items: center;
}

.title-text {
  padding-right: 16px;
}

.header-button {
  height: 32px;
  width: 80px;
  background-color: #FFFFFF;
  border-radius: 4px;
  color: #007BC3;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
}

.page-sub-header {
  display: flex;
  justify-content: space-between;
  padding: 8px 24px;
  width: 100%;
}

.right-button-group {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px
}

.left-button-group {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px
}

.status-as-is {
  border: 1px solid #DCDCDC;
  padding: 6px 12px;
  border-radius: 30px;
}

.status-to-be {
  background-color: #007BC3;
  color: #FFFFFF;
  padding: 6px 12px;
  border-radius: 30px;
}
</style>