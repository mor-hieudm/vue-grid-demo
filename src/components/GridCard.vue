<template>
  <div class="container">
    <div class="card-header">
      <div class="header-title">{{ label }}</div>
      <EditIcon />
    </div>
    <div class="card-body">
      <div v-for="item in score" :key="item.id" class="body-item">
        <div class="mark-context">{{ item.mark }}</div>
        <div class="mark-sub-context">
          <div>{{ item.number }}名</div>
          <div>({{ item.percent }})</div>
        </div>
      </div>
    </div>
    <div class="card-footer">
      <div>{{ totalNum }}名</div>
      <div class="footer-sub-title">（20.0%）</div>
    </div>
  </div>
</template>
<script>
export default {
  name: "GridCard",
  props: {
    label: String,
    score: { type: Array },
    totalNum: Number
  }
}
</script>
<style>
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  border-radius: 6px;
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

.card-body {
  flex: 1;
  border-left: 1px solid #DCDCDC;
  border-right: 1px solid #DCDCDC;
}

.body-item {
  display: flex;
  flex-direction: row;
  padding: 8px 16px;
  justify-content: space-between;
}

.card-footer {
  display: flex;
  justify-content: flex-end;
  align-items: baseline;
  padding-right: 16px;
  padding-bottom: 5px;
  padding-top: 8px;
  border: 1px solid #DCDCDC;
  border-end-start-radius: 6px;
  border-end-end-radius: 6px;
}

.mark-sub-context {
  display: flex;
  width: 83px;
  justify-content: space-between;
  font-size: 10px;
  color: #666666;

}

.footer-sub-title {
  font-size: 10px;
}
</style>