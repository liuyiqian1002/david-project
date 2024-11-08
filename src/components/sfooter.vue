<template>
  <div>
    <div class="footer">
      <div class="logo">
        <img src="../assets/pc/组 11794@3x.png" alt="" />
      </div>
      <div class="main-footer">
        <div class="link-box">
          <div class="frist c">
            <p class="title">Trading market</p>
            <p class="text">market condition</p>
            <p class="text">Spot</p>
            <p class="text">option</p>
          </div>
          <div class="second c">
            <p class="title">Asset</p>
            <p class="text">My assets</p>
            <p class="text">Recharge</p>
            <p class="text">Withdrawal</p>
            <p class="text">Transfer</p>
            <p class="text">Order Center</p>
          </div>
          <div class="third c">
            <p class="title">Product</p>
            <p class="text">Invitation with Gifts</p>
            <p class="text">Platform Introduction</p>
          </div>
          <div class="four c">
            <p class="title">Support</p>
            <p class="text">Beginner Tutorial</p>
            <p class="text">frequently asked questions</p>
          </div>
          <div class="five c">
            <p class="title">Policy</p>
            <p class="text">Term of service</p>
            <p class="text">Privacy Agreement</p>
            <p class="text">Anti Money Laundering Protocol</p>
          </div>
        </div>
        <div class="qrcode-box">
          <div class="code">
            <img :src="appDownloadUrl.qrcode ? appDownloadUrl.qrcode : require(`../assets/pc/组 6155@3x.png`)" alt="" />
          </div>
          <div class="download-btn">
            <div class="download-app">
              <div class="google" @click="downloadApp(1)">
                <img src="../assets/pc/组 3@3x.png" alt="" />
                <p>Google Play</p>
              </div>
              <div class="apple" @click="downloadApp(2)">
                <img src="../assets/pc/路径 5@3x.png" alt="" />
                <p>Apple Store</p>
              </div>
            </div>
          </div>
        </div>
        <div class="desc-box">
          <p class="text">
            All rights reserved @ 2024 sst.com A full-time virtual currency
            service company, sincere duty, service first.
          </p>
          <p class="text">customer service: +66 123456789</p>
          <p class="text">VIPServe: chengkundovl2@gmail.com</p>
        </div>
      </div>
    </div>
    <!-- <search  @changeSt='closrSearch' :nShow = nShow /> -->
  </div>
</template>

<script>
import { Tabbar, TabbarItem, Popup } from "vant";
import config from "@/config";
import search from "@/views/login";
export default {
  name: "tabBar",
  components: {
    [Tabbar.name]: Tabbar,
    [TabbarItem.name]: TabbarItem,
    [Popup.name]: Popup,
    search,
  },
  data() {
    return {
      bar: [],
      active: 0,
      nShow: false,
      appDownloadUrl: config.appDownloadUrl,
    };
  },
  mounted() {
    this.nShow = true;
    this.bar = [
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
    let _this = this;
    _this.bar.forEach((item, index) => {
      if (item.link === _this.$route.path) {
        console.log(_this.$route.path);
        _this.active = index;
      }
    });
  },
  methods: {
    barClick(index, item) {
      this.active = index;
      this.$router.push(item.link);
      window.scrollTo(0, 0);
    },
    closrSearch() {
      this.nShow = !this.nShow;
    },
  downloadApp(type) {
    let downloadUrl = ""
    const { appDownloadUrl } = config;
    console.log(downloadUrl, appDownloadUrl);
    if (type == 1) {
      // android 下载地址
      downloadUrl = appDownloadUrl.android || "https://www.google.com";
    } else {
      // ios 下载地址
      downloadUrl = appDownloadUrl.ios || "https://www.apples.com";
    }
    
    window.open(downloadUrl);
  },
  },
};
</script>
<style lang="less" >
.footer {
  color: #fff;
  margin: 50px 0 0 100px;
  .logo {
    margin: 20px 0 0 0;
  }
  .main-footer {
    display: flex;
    justify-content: space-between;
    .link-box {
      display: flex;
      justify-content: flex-start;
      align-items: flex-start;
      margin-top: 30px;
      flex-grow: 0.8;
      .c {
        width: 20%;
        &:nth-of-type(1) {
        }
      }
      .title {
        font-size: 24px;
        margin-bottom: 20px;
      }
      .text {
        font-size: 18px;
        margin-bottom: 36px;
        width: 140px;
        height: 20px;
        font-family: Source Han Sans CN;
        color: #9e9e9e;
        text-align: left;
        line-height: 36px;
        opacity: 1;
      }
    }
    .qrcode-box {
      flex-grow: 0.4;
      .code {
        width: 180px;
        height: 180px;
        background: #fff;
        margin: 0 auto;
        margin-bottom: 20px;
        img {
          width: 100%;
        }
      }
      .download-app {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        .google,
        .apple {
          width: 200px;
          height: 56px;
          line-height: 56px;
          background: rgba(0, 0, 0, 0.26);
          border: 1px solid #373737;
          border-radius: 10px;
          opacity: 1;
          display: flex;
          justify-content: flex-start;
          align-items: center;
          color: @font_3_color;
          img {
            width: 40px;
            height: 40px;
            margin: 0 20px;
          }
        }
        .google {
          background: #00a770;
          border: 1px solid #73e792;
          border-radius: 10px;
          opacity: 1;
        }
        .apple {
          margin-left: 20px;
        }
      }
    }
    .desc-box {
      flex-grow: 0.4;
      width: 500px;
      text-align: right;
      padding-right: 100px;
      color: #9e9e9e;
      p {
        margin: 20px 0;
      }
    }
  }
}
</style>