<template>
  <div class="market_wrap">
    <sheader></sheader>
    <div class="content-wr">
      <div class="list-wrapper">
        <van-tabs
          @change="changeNav"
          swipeable
          animated
          :border="false"
          color="#fff"
          line-width="40"
          title-active-color="#65BD8D"
        >
          <van-tab
            v-for="(i, index) in navListL"
            :title="i.ItemValue"
            :key="index"
          >
            <div class="content">
              <!-- <van-list
                        v-model="loading"
                        :finished="finished"
                        finished-text="没有更多了"
                        @load="onLoad"
                    > -->
              <ul
                v-for="(i, index) in SymbolsList"
                @click="toKline(i)"
                :key="index"
              >
                <li>
                  <div>
                    <p>
                      {{ i.BaseCurrency }}<span>/{{ i.QuoteCurrency }}</span>
                    </p>
                    <p>24H {{ i.Amount | formatUnit }}</p>
                  </div>
                </li>
                <li>
                  <div>
                    <p>{{ i.ClosePrice }}</p>
                    <p>≈${{ i.Buy1Price | decimals(2) }}</p>
                  </div>
                </li>
                <li>
                  <div
                    :class="[
                      { down: i.RisingValue < 0 },
                      { up: i.RisingValue >= 0 },
                    ]"
                  >
                    {{ gainRate(i) }}%
                  </div>
                </li>
              </ul>
            </div>
          </van-tab>
        </van-tabs>
      </div>
      <div class="k-line-wrapper">
        <contractTrad :symbolId="symbolId" />
      </div>
    </div>
    <search @changeSt="closrSearch" Flag="2" @search="search" :nShow="nShow" />
    <sfooter />
  </div>
</template>

<script>
import contractTrad from "./contractTrad";
import { GetSymbolsArea, GetSymbolsPageList } from "@/api";
import {
  Icon,
  NavBar,
  IndexBar,
  Picker,
  Popup,
  Tab,
  Tabs,
  Empty,
  List,
  Sticky,
} from "vant";
export default {
  name: "market",
  components: {
    [Icon.name]: Icon,
    [IndexBar.name]: IndexBar,
    [NavBar.name]: NavBar,
    [Picker.name]: Picker,
    [Popup.name]: Popup,
    [Empty.name]: Empty,
    [Tab.name]: Tab,
    [Tabs.name]: Tabs,
    [Popup.name]: Popup,
    [Sticky.name]: Sticky,
    [List.name]: List,
    contractTrad,
  },
  data() {
    return {
      nShow: false,
      navListL: [{}],
      navIndex: 0,
      loading: false,
      finished: false,
      refreshing: false,
      SymbolsList: [],
      symbolId: "",
      Time: "",
      AreaId: "Default",
      PageIndex: 1,
      time: "",
      lang: "",
    };
  },
  mounted() {
    this.lang = localStorage.getItem("MINT_lang");
    this.getNav();
    this.getSymbolsPageList();
    this.time = setInterval((_) => {
      this.getSymbolsPageList();
    }, 6000);
    setTimeout((_) => {
      // 创建文本节点
      let html = ` <ul class="nav">
                    <li>
                        ${this.$t("market.name")}
                    </li>
                    <li>
                       ${this.$t("market.closePrice")}
                    </li>
                    <li>
                       ${this.$t("market.gainRate")}
                    </li>
                </ul>`;
      // $('.van-sticky').append(html)
      var para = document.createElement("div");
      para.id = "Nav";
      var element = document.getElementsByClassName("van-sticky");
      element[0].appendChild(para);
      document.getElementById("Nav").innerHTML = html;
    }, 500);
  },
  methods: {
    search(i) {
      this.nShow = !this.nShow;
      this.$toast.loading({
        duration: 0, // 持续展示 toast
        forbidClick: true,
      });
      setTimeout((_) => {
        this.$toast.clear();
        this.toKline(i);
      }, 2000);
    },
    getNav() {
      GetSymbolsArea({}).then((r) => {
        if (r.data.Code == 200) {
          console.log(r.data);
          this.navListL = r.data.Data;
        }
      });
    },
    changeNav(i) {
      console.log(this.navListL[i], "this.navListL[i]");
      let ItemDetailId = this.navListL[i].ItemDetailId;
      console.log(ItemDetailId);
      this.AreaId = ItemDetailId;
      this.SymbolsList = [];
      // this.symbolId = ItemDetailId;
      this.PageIndex = 1;
      this.finished = false;
      clearInterval(this.time);
      this.getSymbolsPageList();
      this.time = setInterval((_) => {
        this.getSymbolsPageList();
      }, 6000);
    },

    onLoad() {
      setTimeout(() => {
        this.PageIndex++;
        this.getSymbolsPageList();
      }, 1000);
    },
    //region 行情 => 获取全部symbol的交易行情列表
    getSymbolsPageList() {
      let data = {
        PageIndex: this.PageIndex,
        PageSize: 100,
        AreaId: this.AreaId,
        Sidx: "SortCode",
        Sord: "desc",
        KeyWord: "Default",
        Flag: "2",
      };
      GetSymbolsPageList(data).then((r) => {
        console.log(r.data);
        if (r.data.Code == 200) {
          this.SymbolsList = r.data.Data.DataList;
          this.symbolId = this.symbolId || this.SymbolsList[0].SymbolsId;
          let Symbols = btoa(JSON.stringify(this.SymbolsList));
          localStorage.setItem("Symbols", Symbols);
          this.loading = false;
        } else {
          this.finished = true;
          if (this.SymbolsList.length == 0) {
            this.isEmpty = true;
          } else {
            this.isEmpty = false;
          }
        }
      });
    },
    gainRate(i) {
      let str = i.RisingValue > 0 ? `+` : ``;
      let num = (((i.ClosePrice - i.OpenPrice) / i.OpenPrice) * 100).toFixed(2);
      return str + num;
    },
    closrSearch() {
      this.nShow = !this.nShow;
    },
    toKline(i) {
      localStorage.setItem("SymbolsIdTiem", JSON.stringify(i));
      localStorage.setItem("path", "deal");
      this.symbolId = i.SymbolsId;
      // this.symbol = i;
      // this.$router.push({
      //   path: "/trad",
      //   query: {
      //     SymbolsId: i.SymbolsId,
      //   },
      // });
    },
  },
  destroyed() {
    console.log("定时器关");
    clearInterval(this.time);
  },
};
</script>
<style lang="less" >
body {
  background-color: @bg_color;
}

.van-hairline--bottom:after {
  border-bottom-width: 0px;
}

.market_wrap_main {
  background-color: @background-linear;
}
// .van-nav-bar{
//   background: @background-linear;
// }

.market_wrap {
  .content-wr {
    display: flex;
    justify-content: flex-start;
    margin: 40px 0 0 0;
    .list-wrapper {
      width: 500px;
      margin-left: 30px;
    }
    .k-line-wrapper {
      width: 1100px;
      margin-left: 40px;
    }
  }
  .van-tabs__wrap {
    background: @bg2_color;
  }
  .van-tabs__nav {
    background: transparent;
    .van-tab {
      background: transparent;
      .van-tab {
        color: @font_color !important;
      }
    }
  }

  .van-tabs__line {
    position: absolute;
    left: 0;
    color: @font_1_color;
    z-index: 1;
    opacity: 0;
  }

  .van-tab__text {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    color: @font_color;
  }
  .nav {
    background-color: @bg2_color;
    display: flex;
    justify-content: space-between;
    padding: 20px;
    .bot_br;
    margin-top: -1px;
    li {
      font-size: 18px;
      color: @font_color;
    }
  }
  .van-tab--active {
    .van-tab__text {
      font-weight: bold;
      font-size: 17px;
      color: #fff !important;
      // transform: scale(0.6);
      border-radius: 28px;
      // background: @btn-background-linear;
      border-bottom: 3px solid @btn-background-linear;
    }
  }
}
</style>
<style lang="less" scoped>
.market_wrap {
  height: 100vh;

  // .nav{
  //     background-color: #fff;
  //     display: flex;
  //     justify-content: space-between;
  //     padding: 20px;
  //     .bot_br;
  //     margin-top: -1px;
  //     li{
  //         font-size: 24px;
  //         color: @sub-font-color;
  //     }
  // }
  .content {
    // padding: 80px 20px 120px 20px;
    // background-color: @bg_color;
    ul {
      display: flex;
      justify-content: space-between;
      margin-bottom: 2px;
      padding: 10px 0;
      align-items: center;
      border-bottom: 1px solid #252525;
      // background: @bg2_color;
      .bot_br;
      li {
        width: 33.33%;
        &:nth-of-type(1) {
          div {
            p {
              &:nth-of-type(1) {
                font-weight: 500;
                font-size: 15px;
                color: @font_3_color;
                span {
                  font-size: 12px;
                  color: @font_color;
                  margin-left: 5px;
                }
              }
              &:nth-of-type(2) {
                font-weight: 400;
                // color: @minor-font-color;
                color: @main_color;
              }
            }
          }
        }
        &:nth-of-type(2) {
          p {
            &:nth-of-type(1) {
              font-weight: 500;
              font-size: 15px;
              color: @font_3_color;
            }
            &:nth-of-type(2) {
              font-weight: 600;
              color: @minor-font-color;
              font-size: 12px;
            }
          }
          text-align: center;
        }
        &:nth-of-type(3) {
          div {
            width: 100px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: @btn_font_color;
            border-radius: 4px;
            &.up {
              background-color: @up-color;
            }
            &.down {
              background-color: @down-color;
            }
          }
          display: flex;
          justify-content: flex-end;
          text-align: right;
        }
      }
    }
  }
}
</style>