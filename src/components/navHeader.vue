<template>
  <div class="nav-header">
    <div class="header">
      <div class="left" @click="$router.go(-1)"><</div>
      <div class="right" @click="$emit('rightClick')">
        {{ rightTitle }}
      </div>
    </div>
  </div>
</template>

<script>
import { Empty } from "vant";
export default {
  name: "empty",
  props: {
    rightTitle: String,
  },
};
</script>

<style scoped lang="less">
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: @font_3_color;
  padding: 20px 0 0 50px;
  .left,
  .right {
    width: 160px;
    height: 50px;
    // background: @btn-color;
    cursor: pointer;
  }
  .left {
    font-size: 30px;
  }
}
</style>
