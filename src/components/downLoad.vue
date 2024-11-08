<template>
  <div class="down_load_wrap" @click="downloadApp">
    <div class="l">
      <img src="../assets/images/common/logo.png" alt="" />
      <div>
        <p>{{ $t("base.title") }}</p>
        <p>{{ $t("base.downloadSubTitle") }}</p>
      </div>
    </div>
    <div class="center">
      <li class="nav-item" :class="{ active: bar.barTitle == title }" v-for="bar in bars" @click="toLink(bar.link)" :key="bar">
        {{ bar.barTitle }}
      </li>
    </div>
    <div class="r">
      <img
        src="../assets/images/common/download1.png"
        class="down_load_ico"
        alt=""
      />
      <img
        @click.stop="closeDownLoad"
        src="../assets/images/common/close_download.png"
        class="close_ico"
        alt=""
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bars: [],
      activeTitle: this.$t("base.home"),
    }
  },
  mounted() {
    this.bars = [
      {
        barTitle: this.$t("base.home"),
        icon: {
          active: require("../assets/images/tab-bar/home_1.png"),
          inactive: require("../assets/images/tab-bar/home_0.png"),
        },
        link: "/",
      },
      {
        barTitle: this.$t("base.market"),
        icon: {
          active: require("../assets/images/tab-bar/market_1.png"),
          inactive: require("../assets/images/tab-bar/market_0.png"),
        },
        link: "/market",
      },
      {
        barTitle: this.$t("base.deal"),
        icon: {
          active: require("../assets/images/tab-bar/trade_1.png"),
          inactive: require("../assets/images/tab-bar/trade_0.png"),
        },
        link: "/deal",
      },
      {
        barTitle: this.$t("base.contract"),
        icon: {
          active: require("../assets/images/tab-bar/futures_1.png"),
          inactive: require("../assets/images/tab-bar/futures_0.png"),
        },
        link: "/contract",
      },
      {
        barTitle: this.$t("base.property"),
        icon: {
          active: require("../assets/images/tab-bar/assets_1.png"),
          inactive: require("../assets/images/tab-bar/assets_0.png"),
        },
        link: "/property",
      },
    ];
  },
  methods: {
    downloadApp() {
      localStorage.setItem("download", Date.now());
      let lang = localStorage.getItem("MINT_lang");
      location.href = `https://download.unltee.com`;
    },
    closeDownLoad() {
      localStorage.setItem("download", Date.now());
      this.$emit("closeDownLoad", false);
    },
    toLink(link) {
      this.$router.push(link);
    }
  },
};
</script>
<style lang="less" scoped>
.down_load_wrap {
  background: @panel_color;
  position: fixed;
  z-index: 100;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
  height: 100px;
  width: 100%;
  height: 71px;
  border-radius: 0px;
  // opacity:0.5;
  // margin: 0;
  > .l {
    display: flex;
    align-items: center;
    > img {
      width: 56px;
    }
    > div {
      color: #1ceddc;
      margin-left: 20px;
      p {
        &:nth-of-type(1) {
          font-size: 20px;
          font-weight: 500;
        }
        &:nth-of-type(2) {
          font-size: 16px;
        }
      }
    }
  }
  > .center{ 
    display: flex;
    justify-content: space-between;
    align-items: center;
    justify-self: flex-start;
    color: @font_color;
    font-size: 18px;
    align-items: center;
    margin-left: -200px;
    li{
      margin-right: 40px;
    }
    .active{
      color: @main_color;
    }
  }
  > .r {
    display: flex;
    align-items: center;
    .down_load_ico {
      width: 56px;
      margin-right: 22px;
    }
    .close_ico {
      width: 26px;
    }
  }
}
</style>