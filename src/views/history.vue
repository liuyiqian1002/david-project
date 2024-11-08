<template>
  <div class="history_wrap">
    <!-- <van-nav-bar
      :title="$t('history.title')"
      left-text
      left-arrow
      :placeholder="true" 
      fixed
      @click-left="$router.go(-1)"
    /> -->
    <div style="height: 1000px">
      <div class="cont">
        <van-tabs
          v-model="active"
          swipeable
          animated
          :offset-top="46"
          color="#1989fa"
          @change="changeNav"
          title-active-color="linear-gradient( 180deg, #5d7de9 0%, #1be5da 100%)"
        >
          <van-tab :title="i" v-for="(i, index) in NavBar" :key="index">
            <div class="position">
              <van-list
                v-model="loading"
                :finished="finished"
                :finished-text="isEmpty ? '' : $t('base.noMore')"
                @load="onLoad"
              >
                <van-empty
                  v-if="isEmpty"
                  :description="$t('base.noData')"
                  class="custom-image"
                  :image="emptyImg"
                />

                <div
                  v-else
                  class="card"
                  v-for="(i, index) in positionEntity"
                  :key="index"
                >
                  <ul>
                    <li>
                      <span>{{ $t("history.createdate") }}:</span
                      >{{ $moment(i.createdate).format("YYYY/MM/DD  HH:mm") }}
                    </li>
                    <li>
                      <span>{{ $t("history.basecurrency") }}:</span
                      >{{ i.basecurrency }}/{{ i.quotecurrency }}
                    </li>
                  </ul>
                  <ul>
                    <li>
                      <span>{{ $t("history.price") }}:</span>{{ i.price }}
                    </li>
                    <li>
                      <span>{{ $t("history.quantity") }}:</span>{{ i.quantity }}
                    </li>
                  </ul>
                </div>
              </van-list>
            </div>
          </van-tab>
        </van-tabs>
      </div>
    </div>
  </div>
</template>

<script>
import {
  Icon,
  NavBar,
  IndexBar,
  Picker,
  Popup,
  Tab,
  Tabs,
  PasswordInput,
  Empty,
  List,
  Sticky,
} from "vant";
import { GetCouplingOrdersPageList } from "@/api";
export default {
  name: "entrust",
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
  },
  data() {
    return {
      active: 0,
      MINT_auth: {},
      SymbolsId: "",
      positionEntity: [],
      NavBar: [],
      PageIndex: 0,
      loading: false,
      finished: false,
      refreshing: false,
      emptyImg: "",
      isEmpty: false,
    };
  },
  mounted() {
    this.emptyImg = require("../assets/images/common/empty_img.png");
    this.NavBar = [this.$t("base.bay"), this.$t("base.sell")];
    let MINT_auth = localStorage.getItem("MINT_auth");
    MINT_auth = this.$decryptByDES(MINT_auth);
    this.MINT_auth = JSON.parse(MINT_auth);
    this.SymbolsId = this.$route.query.SymbolsId;
    this.onLoad();
  },
  methods: {
    changeNav(i) {
      this.isEmpty = false;
      this.positionEntity = [];
      this.PageIndex = 1;
      this.finished = false;
      this.getCouplingOrdersPageList(i + 1);
    },
    //   合约交易 => 获取我的单个合约的持仓信息
    getCouplingOrdersPageList(Category) {
      let data = {
        UserId: this.MINT_auth.UserId,
        Category: Category, //委托方向(1-买入、2-卖出)<br/>
        PageSize: 10, //每页数</br>
        PageIndex: this.PageIndex, //当前页</br>
      };
      GetCouplingOrdersPageList(data).then((r) => {
        console.log(r.data);
        if (r.data.Code == 200) {
          this.positionEntity.push(...r.data.Data);
          this.loading = false;
        } else {
          this.finished = true;
          if (this.positionEntity.length == 0) {
            this.isEmpty = true;
          } else {
            this.isEmpty = false;
          }
        }
      });
    },
    onLoad() {
      setTimeout(() => {
        this.PageIndex++;
        this.getCouplingOrdersPageList(1);
      }, 1000);
    },
  },
};
</script>
<style lang="less" >
body {
  background-color: @bg_color;
}
.history_wrap {
  background-color: @bg_color;
  .custom-image .van-empty__image {
    width: 480px;
    height: 298px;
    margin-top: unset;
  }
  .van-tabs__wrap {
    padding: 20px 0;
    background: #fff;
  }
  // .van-tabs__nav--card {
  //   border: unset;
  // }
  .van-tab {
    width: 190px !important;
    flex: unset;
  }
  // .van-tabs__nav--card {
  //   height: 80px;
  //   display: flex;
  //   justify-content: center;
  // }
  .van-tab {
    line-height: 80px;
    width: 180px;
  }
  .van-tabs__nav {
    .van-tab:nth-of-type(1) {
      border-radius: 5px 0 0 5px;
    }
    .van-tab:nth-of-type(2) {
      border-radius: 0 5px 5px 0;
    }
  }
  .van-tabs__wrap {
    height: 80px;
    width: 400px;
    margin: 0 auto;
  }
  .van-tabs__wrap {
    background-color: @bg_color;
    .van-tab.van-tab--active {
      color: #666;
      background: @background-linear !important;
      color: #fff !important;
    }
    .van-tab {
      background: unset;
      border: 1px solid @main_color !important;
      background: @panel_color !important; // @main_color;
      color: @font_color !important;
      border-color: @sub-font-color !important;
    }
    .van-tabs__line {
      display: none;
    }
  }
  .pic {
    display: flex;
    align-items: center;
    justify-content: center;
    #qrcode {
      img {
        width: 100%;
        height: 100%;
      }
    }
  }
}
</style>
<style lang="less" scoped>
.cont {
  //   border-top: 10px solid #eee;
  .open_position {
    padding: 20px;
    .open_position_cont {
      display: flex;
      padding: 20px;
      > div {
        // .t;
        &.con_left {
          width: 60%;
          .title {
            display: flex;
            justify-content: space-between;
            height: 80px;
            > div {
              flex: 1;
              text-align: center;
              font-size: 28px;
              line-height: 80px;
              color: @sub-font-color;
              border: 1px solid #eee;
              &:nth-of-type(1) {
                &.active {
                  background-color: @up-color;
                  color: #fff;
                }
              }
              &:nth-of-type(2) {
                &.active {
                  background-color: @down-color;
                  color: #fff;
                }
              }
            }
          }
          .txt {
            margin-top: 20px;
            p {
              color: @minor-font-color;
              padding: 10px 0;
            }
            > div {
              margin: 20px 0;
              height: 80px;
              width: 100%;
              border-radius: 4px;
              display: flex;
              justify-content: space-between;
              border: 1px solid #eee;
            }
            .in {
              margin: 0 auto;
              input {
                height: 100%;
                text-align: center;
              }
            }
            .add,
            .minus {
              width: 80px;
              height: 80px;
            }
            .minus {
              background: url(../assets/images/common/iv_open_minus.png)
                no-repeat center;
              background-size: 40%;
            }
            .add {
              background: url(../assets/images/common/iv_open_plus.png)
                no-repeat center;
              background-size: 40%;
            }
          }
          .bot {
            .tips {
              color: @sub-font-color;
              padding: 10px 0;
            }
            .btns {
              height: 80px;
              line-height: 80px;
              display: flex;
              justify-content: space-between;
              &.bay {
                .t;
                .active {
                  background-color: @up-color;
                  color: #fff;
                }
              }
              &.sell {
                .active {
                  background-color: @down-color;
                  color: #fff;
                }
              }
              button {
                width: 48%;
                border-radius: 4px;
                background-color: #eee;
                color: @sub-font-color;
              }
            }
          }
        }
        &.con_right {
          width: 40%;
          padding: 10px;
          p.bay {
            color: @up-color;
            font-size: 26px;
            font-weight: 500;
          }
          p.sell {
            color: @down-color;
          }
          p.sub-color {
            color: @sub-font-color;
          }
          ul {
            display: flex;
            justify-content: space-between;
            &.title {
              color: @sub-font-color;
            }
            .bay {
              color: @up-color;
            }
            .sell {
              color: @down-color;
            }
            li {
              &:nth-of-type(2) {
                color: @sub-font-color;
              }
            }
          }
        }
      }
    }
    > .btns {
      &.bay {
        button {
          background-color: @up-color;
        }
      }
      &.sell {
        button {
          background-color: @down-color;
        }
      }
    }
  }
  .position {
    // padding: 20px;
    .card {
      margin: 0 auto 20px auto;
      padding: 20px;
      background: @panel_color;
      .bot_br;
      ul {
        display: flex;
        flex-wrap: wrap;
        li {
          width: 50%;
          margin: 10px 0;
          color: @font_3_color;
          span {
            color: @main2_color;
            margin-right: 10px;
          }
          &:nth-of-type(3n) {
            text-align: right;
          }
          p {
            padding: 10px 0;
            font-size: 26px;
            &:nth-of-type(1) {
              color: @sub-font-color;
            }
            &:nth-of-type(2) {
              font-weight: 600;
            }
          }
        }
        li:nth-of-type(2) {
          padding-left: 20px;
        }
      }
      .btns {
        margin: 0 auto;
        width: 60%;
        button {
          height: 90px;
          line-height: 90px;
        }
      }
    }
  }
}
</style>