<template>
  <div class="contract_trad_main">
    <div class="top">
      <!-- <van-nav-bar
        :placeholder="true"
        fixed
      >
        <template #right>
          <van-icon
            :name="require('../assets/images/common/ic_kline_w.png')"
            size="18"
            @click="toKline"
          />
          <van-icon
            :name="require('../assets/images/common/ic_pup_w.png')"
            size="18"
            style="margin-left: 10px;"
            @click="showMore = !showMore"
          />
        </template>
        <template #left>
          <van-icon @click="$router.go(-1)" name="arrow-left" size='18' style="margin-right: 10px;" />
          <div @click="showMoreSymbols = !showMoreSymbols">
            {{ title }}<van-icon
            :name="require('../assets/images/common/contract_trad_title_w.png')"
            size="18"
          />
          </div>
        </template>
      </van-nav-bar> -->
      <div class="top-icon">
        <ul>
          <!-- <li>
            <van-icon
              :name="require('../assets/images/common/ic_kline_w.png')"
              size="18"
              @click="toKline"
            />
          </li> -->
          <li @click="toKline">
            <img src="../assets/pc/路径 19232@3x.png" alt="" />
            <p>{{ $t("contractTrad.tokline") }}</p>
          </li>
          <li @click="toExchange(1)">
            <img src="../assets/images/common/ic_transfer_in.png" alt="" />
            <p>{{ $t("contractTrad.cut-in") }}</p>
          </li>

          <li @click="toExchange(2)">
            <img src="../assets/images/common/ic_transfer_out.png" alt="" />
            <p>{{ $t("contractTrad.layOff") }}</p>
          </li>
          <li @click="toEntrust">
            <img src="../assets/images/common/ic_commison.png" alt="" />
            <p>{{ $t("contractTrad.entrust") }}</p>
          </li>
        </ul>
        <!-- <div class="top-more" v-show="showMore" @click.stop="showMore = false">
          <div class="more-con">
            <ul>
              <li @click="toExchange(1)">
                <img src="../assets/images/common/ic_transfer_in.png" alt="" />
                {{ $t("contractTrad.cut-in") }}
              </li>

              <li @click="toExchange(2)">
                <img src="../assets/images/common/ic_transfer_out.png" alt="" />
                {{ $t("contractTrad.layOff") }}
              </li>
              <li @click="toEntrust">
                <img src="../assets/images/common/ic_commison.png" alt="" />
                {{ $t("contractTrad.entrust") }}
              </li>
            </ul>
          </div>
        </div> -->
      </div>
    </div>

    <!-- <div
      class="more_symbols"
      v-show="showMoreSymbols"
      @click="showMoreSymbols = false"
    >
      <div class="more-con">
        <ul
          v-for="i in search"
          :class="{ active: i.SymbolsId == SymbolsId }"
          :key="i.SymbolsId"
          @click="selSymbol(i)"
        >
          <li>{{ i.BaseCurrency }}/USDT</li>
        </ul>
      </div>
    </div> -->
    <div class="contract_trad_wrap">
      <div class="top">
        <p class="title">
          <span>{{ $t("contractTrad.TotalAssets") }}(USDT)</span>
          <span>{{ Accounts.TotalAssets ? Accounts.TotalAssets : "--" }}</span>
        </p>
        <ul>
          <li>
            <p>USDT {{ $t("contractTrad.Balance") }}</p>
            <p>{{ Accounts.Balance ? Accounts.Balance : "--" }}</p>
          </li>
          <li>
            <p>{{ $t("contractTrad.stoplossprice") }} USDT</p>
            <p>{{ Accounts.StopLossPrice ? Accounts.StopLossPrice : "--" }}</p>
          </li>
          <li>
            <p>{{ $t("contractTrad.rofitlossratio") }}</p>
            <p>
              {{
                Accounts.ProfitLossRatio
                  ? (Accounts.ProfitLossRatio * 100).toFixed(2) + "%"
                  : "--"
              }}
            </p>
          </li>
        </ul>
      </div>
      <div class="cont">
        <van-tabs
          v-model="active"
          swipeable
          animated
          :offset-top="46"
          color="#1989fa"
          title-active-color="#03c1a7"
        >
          <van-tab :title="$t('contractTrad.open')">
            <div class="open_position">
              <div class="open_position_cont">
                <div class="con_left">
                  <div class="title">
                    <div
                      @click="tradType = 0"
                      :class="{ active: tradType == 0 }"
                    >
                      {{ $t("contractTrad.many") }}
                    </div>
                    <div
                      @click="tradType = 1"
                      :class="{ active: tradType == 1 }"
                    >
                      {{ $t("contractTrad.empty") }}
                    </div>
                  </div>
                  <div class="txt">
                    <p>{{ $t("contractTrad.Price") }}</p>
                    <div
                      :class="[{ buy: tradType == 0 }, { sell: tradType == 1 }]"
                    >
                      <div class="in">
                        <input type="text" v-model="SymbolsMarket.ClosePrice" />
                      </div>
                    </div>
                  </div>
                  <div class="bay txt">
                    <p>
                      {{ $t("contractTrad.leveragemultiple") }}({{
                        $t("contractTrad.double")
                      }})
                    </p>
                    <div
                      :class="[{ buy: tradType == 0 }, { sell: tradType == 1 }]"
                    >
                      <div class="minus" @click="minus"><div>-</div></div>
                      <div class="in">
                        <input
                          type="text"
                          @blur="changeVal"
                          v-model="LeverageMultiple"
                        />
                      </div>
                      <div class="add" @click="add"><div>+</div></div>
                    </div>
                  </div>
                  <div class="bot">
                    <p class="tips">{{ $t("contractTrad.shippingSpace") }}</p>
                    <!-- <div class="btns bay"> -->
                    <div
                      :class="[
                        'btns',
                        { sell: tradType == 1 },
                        { bay: tradType == 0 },
                      ]"
                    >
                      <button
                        @click="OpenType = 1"
                        :class="{ active: OpenType == 1 }"
                      >
                        {{ $t("contractTrad.all") }}
                      </button>
                      <button
                        @click="OpenType = 2"
                        :class="{ active: OpenType == 2 }"
                      >
                        {{ $t("contractTrad.half") }}
                      </button>
                    </div>
                  </div>
                </div>
                <div class="con_right">
                  <ul class="title">
                    <li>{{ $t("base.strikePrice") }}</li>
                    <li>{{ $t("contractTrad.amount") }}</li>
                  </ul>
                  <ul v-for="i in tradJson.bids">
                    <li class="sell">{{ i[0] | decimals(4) }}</li>
                    <li>{{ i[1] }}K</li>
                  </ul>
                  <p class="bay">
                    {{ SymbolsMarket.ClosePrice | decimals(4) }}
                  </p>
                  <p class="sub-color">
                    ≈{{ SymbolsMarket.ClosePriceCNY | decimals(2) }} USD
                  </p>
                  <ul v-for="i in tradJson.asks">
                    <li class="bay">{{ i[0] | decimals(4) }}</li>
                    <li>{{ i[1] }}K</li>
                  </ul>
                </div>
              </div>
              <div
                :class="[
                  'btns',
                  { sell: tradType == 1 },
                  { bay: tradType == 0 },
                ]"
              >
                <button @click="showPwd" class="main_btn">
                  {{
                    tradType == 0
                      ? $t("contractTrad.many")
                      : $t("contractTrad.empty")
                  }}
                </button>
              </div>
            </div>
          </van-tab>
          <van-tab :title="$t('contractTrad.position')">
            <div class="position">
              <van-empty
                v-show="isPositionEntityEmpty"
                :description="$t('base.noData')"
                class="custom-image"
                :image="emptyImg"
              />
              <div class="card" v-for="i in PositionEntity">
                <div class="title">
                  <div>
                    <span>{{ i.BaseCurrency }}/USDT</span>
                    <!-- 0-开多、1-开空 -->
                    <span
                      :class="[
                        { 'down-color': i.Flag == 1 },
                        { 'up-color': i.Flag == 0 },
                      ]"
                      >{{
                        i.Flag == 1
                          ? $t("contractTrad.empty")
                          : $t("contractTrad.many")
                      }}</span
                    >
                    <span
                      :class="[
                        { 'down-color': i.Flag == 1 },
                        { 'up-color': i.Flag == 0 },
                      ]"
                      >{{ (i.IncomeRate * 100).toFixed(2) + "%" }}</span
                    >
                    <span>{{ $t("contractTrad.incomeRate") }}</span>
                  </div>
                </div>
                <div>
                  <ul>
                    <li>
                      <p>{{ $t("contractTrad.positionnum1") }}(USDT)</p>
                      <p>{{ i.PositionNum }}</p>
                    </li>
                    <li>
                      <p>{{ $t("contractTrad.createprice") }}(USDT)</p>
                      <p>{{ i.CreatePrice }}</p>
                    </li>
                    <li>
                      <p>{{ $t("contractTrad.income") }}(USDT)</p>
                      <p>{{ i.Income }}</p>
                    </li>
                    <li>
                      <p>{{ $t("contractTrad.positionnum") }}(USDT)</p>
                      <p>{{ i.PositionNum }}</p>
                    </li>
                    <li>
                      <p>{{ $t("contractTrad.NowPrice") }}(USDT)</p>
                      <p>{{ i.NowPrice }}</p>
                    </li>
                    <li>
                      <p>{{ $t("contractTrad.fee") }}(USDT)</p>
                      <p>{{ i.Margin }}</p>
                    </li>
                    <li>
                      <p>
                        {{ $t("contractTrad.leveragemultiple") }}({{
                          $t("contractTrad.double")
                        }})
                      </p>
                      <p>{{ i.LeverageMultiple }}</p>
                    </li>
                    <li v-show="CountDownTime(i) > 0">
                      <p>{{ $t("base.CloseDate") }}</p>
                      <p>
                        <van-count-down :time="CountDownTime(i)">
                          <template #default="timeData">
                            <span
                              >{{
                                timeData.days > 10
                                  ? timeData.days
                                  : "0" + timeData.days
                              }}
                              {{ $t("financial.day") }}
                            </span>
                            <span>{{
                              timeData.hours > 10
                                ? timeData.hours
                                : "0" + timeData.hours
                            }}</span>
                            <span>:</span>
                            <span>{{
                              timeData.minutes > 10
                                ? timeData.minutes
                                : "0" + timeData.minutes
                            }}</span>
                            <span>:</span>
                            <span>{{
                              timeData.seconds > 10
                                ? timeData.seconds
                                : "0" + timeData.seconds
                            }}</span>
                          </template>
                        </van-count-down>
                      </p>
                    </li>
                  </ul>
                  <div class="btns">
                    <button
                      class="main_btn"
                      @click="submitCloseFutures(i.OrderId)"
                    >
                      {{ $t("contractTrad.create") }}
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </van-tab>
        </van-tabs>
      </div>
    </div>
    <van-dialog
      v-model="show"
      :title="$t('login.password')"
      @confirm="submitOpenFutures"
      show-cancel-button
    >
      <!-- <p class="pwd_tips" @click="showKeyboard = !showKeyboard"  v-show="!Password">{{$t('login.password')}}</p> -->
      <ul class="pwd_num" @click="showKeyboard = !showKeyboard">
        <li v-for="i in Password"></li>
        <li class="unent" v-for="i in 6 - Password.length"></li>
      </ul>
    </van-dialog>
    <!-- <van-number-keyboard
      :show="showKeyboard"
      @input="onInput"
      @delete="onDelete"
      @blur="showKeyboard = false"
    /> -->
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
  Empty,
  Sticky,
  Dialog,
  PasswordInput,
  NumberKeyboard,
  CountDown,
} from "vant";
import Vue from "vue";
Vue.use(PasswordInput);
Vue.use(NumberKeyboard);
import {
  GetSymbolsMarketDepth,
  SubmitOpenFutures,
  GetUserFuturesAccountsEntity,
  SubmitCloseFutures,
  GetUserPositionEntity,
  GetUserFuturesHistoryList,
} from "@/api";
export default {
  name: "contractTrad",
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
    [Dialog.name]: Dialog,
    [CountDown.name]: CountDown,
  },
  props: ["symbolId"],
  data() {
    return {
      active: 0,
      tradType: 0, //委托方向(0-开多、1-开空)
      showMore: false,
      SymbolsId: "",
      tradJson: {},
      SymbolsMarket: {},
      MINT_auth: {},
      OpenType: 1, //开仓类型(1-全仓、2-半仓)
      LeverageMultiple: 3,
      showKeyboard: false,
      show: false,
      Password: "",
      Accounts: "",
      PositionEntity: [],
      isPositionEntityEmpty: false,
      emptyImg: "",
      showMoreSymbols: false,
      title: "",
      search: [],
      time: "",
    };
  },
  mounted() {
    this.emptyImg = require("../assets/images/common/empty_img.png");
    // this.SymbolsId = this.$route.query.id;
    this.getSymbolInfo();
  },
  methods: {
    getSymbolInfo(id) {
      // console.log(id,1111)
      let MINT_auth = localStorage.getItem("MINT_auth");
      MINT_auth = this.$decryptByDES(MINT_auth);
      this.MINT_auth = JSON.parse(MINT_auth);
      let search = localStorage.getItem("search")
        ? localStorage.getItem("search")
        : "";
      search = JSON.parse(atob(search));
      this.search = search;
      console.log(search);
      let SymbolsIdTiem = localStorage.getItem("contractSymbolsItem")
        ? JSON.parse(localStorage.getItem("contractSymbolsItem"))
        : "";
      this.SymbolsId =
        id ||
        this.$route.query.SymbolsId ||
        this.symbolId ||
        SymbolsIdTiem.SymbolsId;
      this.SymbolsIdTiem = SymbolsIdTiem;
      search.map((_) => {
        if (_.SymbolsId == this.SymbolsId) {
          this.title = _.BaseCurrency + "/" + _.QuoteCurrency;
          this.SymbolsIdTiem = _;
        }
      });
      this.getSymbolsMarketDepth();
      this.getUserPositionEntity();
      this.getUserFuturesAccountsEntity();

      this.time = setInterval(() => {
        this.getSymbolsMarketDepth();
        this.getUserPositionEntity();
        this.getUserFuturesAccountsEntity();
      }, 6000);
    },
    // 行情 => 获取交易对的买1-买5，
    getSymbolsMarketDepth() {
      let data = {
        SymbolsId: this.SymbolsId,
      };
      GetSymbolsMarketDepth(data).then((r) => {
        if (r.data.Code == 200) {
          this.SymbolsMarket = r.data.Data;
          this.tradJson = JSON.parse(r.data.Data.JsonString);
        }
        console.log(r.data);
      });
    },
    // 合约交易 => 用户提交合约开仓
    submitOpenFutures(action, done) {
      if (!this.Password) {
        // this.$toast('密码不格式不正确');
        return;
      }
      let data = {
        UserId: this.MINT_auth.UserId, //用户ID</br>
        SymbolsId: this.SymbolsId, //交易对主键编号<br/>
        Flag: this.tradType, //委托方向(0-开多、1-开空)<br/>
        LeverageMultiple: this.LeverageMultiple, //杠杆倍数(3-50倍证数)</br>
        OpenType: this.OpenType, //开仓类型(1-全仓、2-半仓)<br/>
        Password: this.Password,
      };
      console.error(data);
      SubmitOpenFutures(data).then((r) => {
        console.error(r);
        if (r.data.Code == 200) {
          this.$toast(r.data.Message);
          this.active = 1;
          this.getUserPositionEntity();
        }
      });
    },
    // 合约交易 => 获取我的单个合约资产
    getUserFuturesAccountsEntity() {
      let data = {
        UserId: this.MINT_auth.UserId, //用户ID</br>
        SymbolsId: this.SymbolsId, //交易对主键编号<br/>
      };
      GetUserFuturesAccountsEntity(data).then((r) => {
        if (r.data.Code == 200) {
          console.log(r.data.Data);
          this.Accounts = r.data.Data;
        }
      });
    },
    showPwd() {
      this.show = true;
      this.showKeyboard = true;
      this.Password = "";
      this.registerKeyboardEvent();
    },

    registerKeyboardEvent() {
      document.addEventListener("keydown", this.onkeydown);
    },
    removeEventListener() {
      document.removeEventListener("keydown", this.onkeydown);
    },
    onkeydown(e) {
      if(/[0-9]/.test(e.key)){
        this.onInput(e.key);
      }
      if (e.key == "Backspace") {
        this.onDelete();
      }
    },
    onInput(key) {
      this.Password = (this.Password + key).slice(0, 6);
      console.log(this.Password);
    },
    onDelete() {
      this.Password = this.Password.slice(0, this.Password.length - 1);
    },

    toExchange(type) {
      let BaseCurrency = this.SymbolsIdTiem.BaseCurrency;
      this.$router.push({
        path: "/exchange",
        query: {
          type,
          BaseCurrency,
          SymbolsId: this.SymbolsId,
        },
      });
    },
    minus() {
      if (this.LeverageMultiple <= 3) {
        this.LeverageMultiple = 3;
        return;
      } else {
        this.LeverageMultiple -= 1;
      }
    },
    add() {
      if (this.LeverageMultiple >= 50) {
        this.LeverageMultiple = 50;
      } else {
        this.LeverageMultiple += 1;
      }
    },
    toEntrust() {
      this.$router.push({
        path: "/entrust",
        query: {
          SymbolsId: this.SymbolsId,
        },
      });
    },
    //   region 合约交易 => 获取我的单个合约的持仓信息
    getUserPositionEntity() {
      this.isPositionEntityEmpty = false;
      let data = {
        UserId: this.MINT_auth.UserId, //用户Id</br>
        SymbolsId: this.SymbolsId, //交易对主键</br>
      };
      GetUserPositionEntity(data).then((r) => {
        console.log(JSON.stringify(r.data));
        // r.data = {"Code":200,"Message":"获取成功","Data":{"OrderId":"9601eb85-9b6b-4700-8401-e1668d0f7776","Code":"2020082221174158732523","BaseCurrency":"MYD","Flag":0,"PositionNum":1547.47235295,"CreatePrice":0.05,"NowPrice":0.05,"Margin":515.82411765,"LeverageMultiple":3,"CreateDate":"2020-08-22 21:17:41.587","Income":0,"IncomeRate":0}}
        console.log(r.data);
        if (r.data.Code == 200) {
          this.PositionEntity = [r.data.Data];
          console.log(this.PositionEntity);
          this.loading = false;
        } else {
          this.PositionEntity = [];
          this.finished = true;
          if (this.PositionEntity.length == 0) {
            this.isPositionEntityEmpty = true;
          } else {
            this.isPositionEntityEmpty = false;
          }
        }
      });
    },
    CountDownTime(i) {
      // i.CloseDate i.NowDate
      let NowDate = i.NowDate ? i.NowDate.replace(/-/g, "/").split(".")[0] : "";
      let CloseDate = i.CloseDate
        ? i.CloseDate.replace(/-/g, "/").split(".")[0]
        : "";
      let leftTimes = new Date(CloseDate) - new Date(NowDate);
      return leftTimes;
    },
    // 平仓
    submitCloseFutures(id) {
      let data = {
        UserId: this.MINT_auth.UserId, //用户ID</br>
        OrderId: id, //持仓订单主键<br/>
      };
      Dialog.confirm({
        title: this.$t("base.confirm"),
        message: this.$t("entrust.confirm"),
      })
        .then(() => {
          // on confirm
          SubmitCloseFutures(data).then((r) => {
            console.log(r.data);
            if (r.data.Code == 200) {
              this.$toast(r.data.Message);
              this.getUserPositionEntity();
            }
          });
        })
        .catch(() => {
          // on cancel
        });
    },
    selSymbol(i) {
      // localStorage.setItem('SymbolsIdTiem',JSON.stringify(i));
      this.SymbolsIdTiem = i;
      this.SymbolsId = i.SymbolsId;
      this.title = i.BaseCurrency + "/" + i.QuoteCurrency;
      this.showMoreSymbols = false;
      this.getSymbolsMarketDepth();
      this.getUserPositionEntity();
      this.getUserFuturesAccountsEntity();
    },
    toKline() {
      localStorage.setItem("SymbolsIdTiem", JSON.stringify(this.SymbolsIdTiem));
      localStorage.setItem("path", "contract");
      const { path } = this.$route;
      const url = `${window.location.origin}/#/trad?SymbolsId=${this.SymbolsId}`;
      console.error(url);
      window.open(url);
      // this.$router.push({
      //   path: "/trad",
      //   query: {
      //     SymbolsId: this.SymbolsId,
      //   },
      // });
    },
    changeVal() {
      this.LeverageMultiple < 3
        ? (this.LeverageMultiple = 3)
        : this.LeverageMultiple > 50
        ? (this.this.LeverageMultiple = 50)
        : "";
    },
  },
  watch: {
    symbolId(val) {
      // this.symbolId = this.symbolId;
      val && this.getSymbolInfo(val);
    },
  },
  destroyed() {
    clearInterval(this.time);
  },
};
</script>
<style lang="less" >
body {
  background-color: @background-color;
}
.contract_trad_main {
  // background-color: @background-color;
  .custom-image .van-empty__image {
    width: 480px;
    height: 298px;
    margin-top: unset;
  }
  // .van-nav-bar {
  //   background-color: transparent;
  // }
  // .van-icon,
  // .van-nav-bar__left {
  //   color: #fff !important;
  // }
  // .van-hairline--top-bottom,
  // .van-hairline--bottom {
  //   &::after {
  //     content: "";
  //     border-bottom-width: 0px;
  //     border: none !important;
  //   }
  // }
  .van-overlay {
    z-index: 10 !important;
  }
  .van-tabs__line {
    background: @main2_color !important;
  }
}
</style>
<style lang="less" scoped>
.contract_trad_main {
  // background-color: @background-color;
  // height: 100vh;
  // display: flex;
  // justify-content: flex-start;
  // align-items: center;
  // flex-direction: column;
  > .top {
    background-size: cover;
    // position: fixed;
    // z-index: 10;
    width: 100%;
  }
  .pwd_tips {
    text-align: center;
    height: 90px;
    // margin: 10px;/*  */
    line-height: 90px;
  }
  .pwd_num {
    display: flex;
    height: 90px;
    justify-content: center;
    align-items: center;
    li {
      color: #fffafa;
      border-radius: 50%;
      width: 30px;
      height: 30px;
      background-color: #ffffff; // __COLOR 密码框颜色//
      margin: 0 10px;
      &.unent {
        margin: 0 10px;
        border: none;
        background-color: transparent;
        position: relative;
        &:before {
          position: absolute;
          content: "";
          display: block;
          width: 30px;
          height: 2px;
          background-color: #ffffff; // __COLOR 密码框颜色//
          top: 50%;
          left: 0;
          transform: translate(0, -50%);
        }
      }
    }
  }
  .top-icon {
    width: 820px;
    ul {
      position: relative;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      li {
        width: 200px;
        height: 50px;
        line-height: 50px;
        text-align: center;
        display: flex;
        justify-content: center;
        align-content: center;
        color: #fff;
        cursor: pointer;
        border: 1px solid #ccc;
        p {
          padding-left: 20px;
        }
        img {
          width: 30px;
          height: 30px;
          margin-top: 10px;
        }
      }
      li:nth-of-type(1) {
        img {
          filter: grayscale(0.5);
          opacity: 0.71;
        }
      }
    }
  }
  // z-index: 10000000;
  .top-more {
    position: absolute;
    left: 10px;
    width: 100%;
    top: 0;
    // bottom: 0;
    height: 200px;
    z-index: 1000;
    .more-con {
      width: 30%;
      background-color: @panel_color;
      color: @font_1_color;
      // position: absolute;
      // right: 20px;
      // z-index: 1000;
      // top: 46px;
      border-radius: 10px;
      ul {
        width: 100%;

        li {
          text-align: center;
          height: 90px;
          line-height: 90px;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: 21px;
          padding: 10px;
          img {
            height: 30px;
            margin-right: 20px;
          }
        }
      }
    }
  }

  // .more_symbols {
  //   position: absolute;
  //   width: 100%;
  //   color: @font_1_color;
  //   top: 0;
  //   right: 0;
  //   left: 0;
  //   bottom: 0;
  //   z-index: 100;
  //   .more-con {
  //     width: 100%;
  //     top: 46px;
  //     height: 560px;
  //     overflow: scroll;
  //     background-color: @panel_color;
  //     position: absolute;
  //     right: 0;
  //     left: 0;
  //     margin: 0 auto;
  //     border-radius: 10px;
  //     ul {
  //       width: 100%;
  //       &.active {
  //         li {
  //           color: @font_1_color;
  //         }
  //       }
  //       li {
  //         text-align: center;
  //         height: 90px;
  //         line-height: 90px;
  //         display: flex;
  //         align-items: center;
  //         justify-content: center;
  //         font-size: 28px;
  //         padding: 10px;
  //         img {
  //           height: 30px;
  //           margin-right: 20px;
  //         }
  //       }
  //     }
  //   }
  // }
  .contract_trad_wrap {
    position: relative;
    top: 20px;
    // top: -120px;
    .top {
      width: 100%;
      // margin: 0 auto;
      padding: 20px;
      color: @font_color;
      background-color: @panel_color;
      border-radius: 24px;
      .title {
        // height: 80px;
        // line-height: 80px;
        .bot_br;
        font-size: 21px;
        span {
          display: block;
          &:nth-of-type(1) {
            color: @font_color;
            margin-right: 20px;
          }
        }
      }
      ul {
        display: flex;
        justify-content: space-between;
        li {
          flex: 1;
          p {
            padding: 10px 0;
            color: @font_color;
          }
          p:nth-of-type(1) {
            color: @font_color;
          }
        }
      }
    }
    .cont {
      // border-top: 10px solid #eee;
      margin-top: 20px;
      .open_position {
        padding: 20px 0;
        .open_position_cont {
          display: flex;
          padding: 20px 0;
          > div {
            &.con_left {
              width: 60%;
              .title {
                display: flex;
                justify-content: space-between;
                height: 50px;
                > div {
                  flex: 1;
                  text-align: center;
                  font-size: 15px;
                  line-height: 50px;
                  color: @sub-font-color;
                  border: 1px solid @sub-font-color;
                  &:nth-of-type(1) {
                    &.active {
                      background-color: @main2_color;
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
              input {
                background-color: @background-color;
                color: @font_1_color;
              }
              .txt {
                margin-top: 10px;
                p {
                  color: @minor-font-color;
                  padding: 10px 0;
                }
                > div {
                  margin: 10px 0;
                  height: 50px;
                  width: 100%;
                  border-radius: 4px;
                  display: flex;
                  justify-content: space-between;
                  border: 1px solid #eee;
                  .minus,
                  .add {
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    font-size: 30px;
                    color: @font_3_color;
                  }
                  &.buy {
                    border: 1px solid @main2_color;
                    .minus {
                      background: @main2_color;
                      // background: url(../assets/images/common/minus_green.png)
                      // background-size: 100%
                    }
                    .add {
                      background: @main2_color;
                      // background: url(../assets/images/common/plus_green.png) no-repeat center;
                      // background-size: 100%;
                    }
                  }
                  &.sell {
                    border: 1px solid @down-color;
                    .minus {
                      background: @down-color;
                      // background: url(../assets/images/common/minus_red.png) no-repeat center;
                      background-size: 100%;
                    }
                    .add {
                      background: @down-color;
                      // background: url(../assets/images/common/plus_red.png) no-repeat center;
                      background-size: 100%;
                    }
                  }
                }
                .in {
                  margin: 0 auto;
                  input {
                    height: 100%;
                    width: 90%;
                    text-align: center;
                  }
                }
                .add,
                .minus {
                  width: 50px;
                  height: 50px;
                }
              }
              .bot {
                .tips {
                  color: @sub-font-color;
                  padding: 10px 0;
                }
                .btns {
                  height: 50px;
                  line-height: 50px;
                  display: flex;
                  justify-content: space-between;
                  &.bay {
                    .active {
                      background-color: @main2_color;
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
                    background-color: @panel_color;
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
                font-size: 14px;
                font-weight: 500;
                text-align: center;
              }
              p.sell {
                color: @down-color;
                text-align: center;
              }
              p.sub-color {
                color: @sub-font-color;
                text-align: center;
              }
              ul {
                display: flex;
                justify-content: space-between;
                margin: 15px 10px;
                &.title {
                  color: @sub-font-color;
                }
                .bay {
                  color: @main2_color;
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
              background-color: @main2_color;
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
        padding: 20px;
        .card {
          margin-bottom: 20px;
          padding: 20px 0;
          .bot_br;
          > div.title {
            color: @font_color;
            height: 40px;
            line-height: 40px;
            span {
              &:not(:nth-last-of-type(1)) {
                font-size: 14px;
                margin-right: 10px;
                font-weight: 500;
              }
              &:nth-last-of-type(1) {
                color: @sub-font-color;
              }
            }
          }
          ul {
            display: flex;
            flex-wrap: wrap;
            li {
              width: 33.33%;
              margin: 10px 0;
              &:nth-of-type(3n) {
                text-align: right;
              }
              p {
                color: @font_color;
                padding: 10px 0;
                font-size: 14px;
                &:nth-of-type(1) {
                  color: @sub-font-color;
                }
                &:nth-of-type(2) {
                  font-weight: 600;
                }
              }
            }
          }
          .btns {
            margin: 0 auto;
            width: 60%;
            button {
              height: 60px;
              line-height: 60px;
            }
          }
        }
      }
    }
  }
}
.van-count-down {
  color: @font_color;
}
</style>