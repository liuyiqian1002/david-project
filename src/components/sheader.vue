<template>
  <div class="s-header">
    <div class="l">
      <img src="../assets/images/common/logo.png" alt="" />
      <div>
        <p>{{ $t("base.title") }}</p>
        <p>{{ $t("base.downloadSubTitle") }}</p>
      </div>
    </div>
    <div class="center">
      <li
        class="nav-item"
        :class="{ active: bar.index == activeIndex }"
        v-for="(bar, index) in bars"
        @click="toLink(bar)"
        :key="index"
      >
        {{ bar.barTitle }}
      </li>
    </div>
    <div class="r">
      <img
        src="../assets/pc/组 4@3x.png"
        class="down_load_ico"
        alt=""
        @click="goToPay"
      />
      <img
        @click.stop="personalInfo"
        src="../assets/pc/路径 6@3x.png"
        class="down_load_ico"
        alt=""
      />
      <img
        @click.stop="onService"
        src="../assets/pc/组 5@3x.png"
        class="down_load_ico"
        alt=""
      />
      <img
        src="../assets/pc/路径 11@3x.png"
        @click="goNews"
        class="down_load_ico"
        alt=""
      />
      <div class="choselanguage">
        <img
          src="../assets/pc/路径 12@3x.png"
          class="down_load_ico"
          alt=""
          @click="showLanguage = !showLanguage"
        />
        <div
          class="language-box"
          v-if="showLanguage"
          @mouseleave="showLanguage = false"
        >
          <setLang @getBars="getBars"></setLang>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import setLang from "@/views/setLan";
export default {
  components: {
    setLang,
  },
  data() {
    return {
      bars: [],
      activeTitle: this.$t("base.home"),
      lang: "en",
      MINT_auth: {},
      showLanguage: false,
      activeIndex: 1,
    };
  },
  computed: {
    ...mapState(["base"]),
  },
  mounted() {
    this.getBars();
    console.log(this.bars, "base");
    this.activeIndex = this.base.activeIndex;
    this.lang = localStorage.getItem("MINT_lang");
    let MINT_auth = localStorage.getItem("MINT_auth");
    MINT_auth = this.$decryptByDES(MINT_auth);
    this.MINT_auth = JSON.parse(MINT_auth);
  },
  methods: {
    getBars() {
      this.bars = [
        {
          barTitle: this.$t("base.home"),
          icon: {
            active: require("../assets/images/tab-bar/home_1.png"),
            inactive: require("../assets/images/tab-bar/home_0.png"),
          },
          link: "/",
          index: 1,
        },
        {
          barTitle: this.$t("base.market"),
          icon: {
            active: require("../assets/images/tab-bar/market_1.png"),
            inactive: require("../assets/images/tab-bar/market_0.png"),
          },
          link: "/market",
          index: 2,
        },
        {
          barTitle: this.$t("base.deal"),
          icon: {
            active: require("../assets/images/tab-bar/trade_1.png"),
            inactive: require("../assets/images/tab-bar/trade_0.png"),
          },
          link: "/deal",
          index: 3,
        },
        {
          barTitle: this.$t("base.contract"),
          icon: {
            active: require("../assets/images/tab-bar/futures_1.png"),
            inactive: require("../assets/images/tab-bar/futures_0.png"),
          },
          link: "/futures",
          index: 4,
        },
        {
          barTitle: this.$t("base.property"),
          icon: {
            active: require("../assets/images/tab-bar/assets_1.png"),
            inactive: require("../assets/images/tab-bar/assets_0.png"),
          },
          link: "/property",
          index: 5,
        },
      ];
    },
    onService() {
      let url =
        this.$config.imgPath +
        `/News/CustomerService/${this.lang}?token=${this.MINT_auth.UserId}`;
      // location.href = url;
      window.open(url);
    },
    personalInfo() {
      this.$router.push("/property");
      this.$store.commit("setActiveIndex", 5);
    },
    toLink(bar) {
      console.log(bar.link);
      this.activeTitle = bar.barTitle;
      this.activeIndex = bar.index;
      this.$store.commit("setActiveIndex", bar.index);
      this.$router.push(bar.link);
    },
    goToPay() {
      // this.$store.commit("setActiveIndex", 5);
      this.$router.push({
        path: "/property",
        query: {
          index: 6,
        },
      });
    },
    goNews() {
      // this.$store.commit("setActiveIndex", 5);
      this.$router.push({
        path: "/property",
        query: {
          index: 7,
        },
      });
    },
  },
};
</script>
<style lang="less" scoped>
.s-header {
  background: rgba(000, 000, 000, 0.5);
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
  > .center {
    display: flex;
    justify-content: space-between;
    align-items: center;
    justify-self: flex-start;
    color: @font_color;
    font-size: 24px;
    align-items: center;
    margin-left: -200px;
    li {
      margin-right: 40px;
      cursor: pointer;
    }
    .active {
      color: @main_color;
    }
  }
  > .r {
    display: flex;
    align-items: center;
    .down_load_ico {
      width: 26px;
      margin-right: 22px;
      cursor: pointer;
    }
    .choselanguage {
      position: relative;

      .language-box {
        height: 200px;
        width: 220px;
        position: absolute;
        top: 50px;
        right: 0;
        z-index: 100;
      }
    }
  }
}
</style>