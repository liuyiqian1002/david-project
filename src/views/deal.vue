<template>
  <!-- src/views/deal.vue -->
  <div class="deal-wrapper">
    <sheader></sheader>
    <div class="deal_wrap">
      <div class="left-nav-wrapper">
        <div
          v-for="item in navList"
          :key="item.val"
          :class="{ active: active == item.val }"
          @click="()=>{active = item.val; item.val !== 3 && $refs.dealBox.selectTradType(item.val)}"
          class="buy"
        >
          <p>{{ item.label }}</p>
        </div>
      </div>
      <div class="right-wrapper">
        <history v-show="active == 3" ref="history"></history>
        <dealBox v-show="active == 1 || active == 2" ref="dealBox"></dealBox>
      </div>
    </div>
    <sfooter></sfooter>
  </div>
</template>

<script>
// import {
//   Icon,
//   NavBar,
//   IndexBar,
//   Picker,
//   Popup,
//   Tab,
//   Tabs,
//   PasswordInput,
//   NumberKeyboard,
//   Empty,
//   Sticky,
// } from "vant";
import Vue from "vue";
// Vue.use(PasswordInput);
// Vue.use(NumberKeyboard);
import dealBox from "./dealBox.vue";
import history from "./history.vue";
export default {
  name: "deal",
  components: {
    // [Icon.name]: Icon,
    // [IndexBar.name]: IndexBar,
    // [NavBar.name]: NavBar,
    // [Picker.name]: Picker,
    // [Popup.name]: Popup,
    // [Empty.name]: Empty,
    // [Tab.name]: Tab,
    // [Tabs.name]: Tabs,
    // [Popup.name]: Popup,
    // [Sticky.name]: Sticky,
    dealBox,
    history
  },
  data() {
    return {
      active: 0,
      navList: [
        {
          label: this.$t("base.bay"),
          val: 1,
        },
        {
          label: this.$t("base.sell"),
          val: 2,
        },
        {
          label: this.$t("deal.record"),
          val: 3,
        },
      ],
    };
  },
  mounted() {
    this.active = this.navList[0].val;
  },
  methods: {},
  watch: {},
};
</script>
<style lang="less" >
.deal_wrap {
  // background-color: @bg_color;
  // height: 100vh;
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  margin-top: 20px;
  .left-nav-wrapper {
    width: 300px;
    display: flex;
    justify-content: flex-start;
    flex-direction: column;
    align-items: center;
    color: @font_3_color;
    margin-top: 100px;
    > div {
      width: 250px;
      height: 50px;
      line-height: 50px;
      font-size: 18px;
      cursor: pointer;
      padding: 0 40px;
    }
    .active {
      border-radius: 10px;
      background: @background-linear;
      color: @font_3_color;
    }
  }
  .right-wrapper {
    width: 900px;
  }
}
.van-overlay {
  z-index: 10 !important;
}
</style>
</style>