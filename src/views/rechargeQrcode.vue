<template>
  <div>
    <!-- <van-nav-bar
      :title="title"
      left-text
      left-arrow
      @click-left="$router.go(-1)"
    /> -->
    <div class="recharge_qrcode_wrap">
      <div class="content">
        <div class="" v-show="coinInfo.Coin == 'USDT'">
          <p class="title">{{ $t("rechargeQrcode.ChainName") }}</p>
          <ul class="chainName">
            <li>
              <div class="btns">
                <button
                  @click="changeNav(index)"
                  :class="['main_btn', { active: btnIndex == index }]"
                  v-for="(i, index) in btns"
                  :key="index"
                >
                  {{ i }}
                </button>
              </div>
            </li>
            <li></li>
          </ul>
        </div>
        <div class="qr">
          <div ref="qrcode" id="qrcode"></div>
        </div>
        <p class="qr_tips">{{ tips }}</p>
        <div class="url">
          <p>{{ codeUrl }}</p>
          <span
            class="mc"
            v-clipboard:copy="codeUrl"
            v-clipboard:success="onCopy"
            v-clipboard:error="onError"
            >{{ $t("rechargeQrcode.copy") }}</span
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Icon, NavBar, IndexBar } from "vant";
import QRCode from "qrcodejs2";
import { GetUserCoindAddress, GetUserCommodityAccountsInfo } from "@/api";
export default {
  name: "rechargeQrcode",
  components: {
    [Icon.name]: Icon,
    [IndexBar.name]: IndexBar,
    [NavBar.name]: NavBar,
  },
  data() {
    return {
      title: "",
      coinInfo: {},
      MINT_auth: {},
      btns: ["ERC20", "TRC20", "OMNI"],
      btnIndex: 0,
      codeUrl: "",
      coin: "",
      tips: "",
    };
  },
  mounted() {
    let MINT_auth = localStorage.getItem("MINT_auth");
    MINT_auth = this.$decryptByDES(MINT_auth);
    this.MINT_auth = JSON.parse(MINT_auth);
    this.getUserCommodityAccountsInfo();
    // this.coin = this.$route.query.coin;
    // this.title = this.coin;
  },
  methods: {
    getRechargeInfo() {
      //   let coinInfo = localStorage.getItem("coinInfo");
      //   coinInfo = this.$decryptByDES(coinInfo);
      //   this.coinInfo = JSON.parse(coinInfo);
      // console.log("coinInfo");
      // console.log(coinInfo);
      if (!this.coinInfo.CoinAddress) {
        this.getUserCoindAddress();
      } else {
        this.codeUrl = this.coinInfo.CoinAddress;
        this.setQRCode();
      }
      let tips = this.$t("rechargeQrcode.tips");
      this.tips = tips.replace(/{coin}/g, this.coin);
    },
    getUserCoindAddress() {
      let ChainName =
        this.coin == "USDT" ? this.btns[this.btnIndex] : "default";
      let data = {
        UserId: this.MINT_auth.UserId,
        Coin: this.coin,
        ChainName,
      };
      GetUserCoindAddress(data).then((r) => {
        console.log(r);
        this.codeUrl = r.data.Data.CoinAddress;
        this.setQRCode();
      });
    },
    changeNav(i) {
      this.btnIndex = i;
      if (i == 0) {
        if (!this.coinInfo.CoinAddress) {
          this.getUserCoindAddress();
        } else {
          this.codeUrl = this.coinInfo.CoinAddress;
          this.setQRCode();
        }
      }
      if (i == 1) {
        if (!this.coinInfo.CoinAddressTRC20) {
          this.getUserCoindAddress();
        } else {
          this.codeUrl = this.coinInfo.CoinAddressTRC20;
          this.setQRCode();
        }
      }
      if (i == 2) {
        if (!this.coinInfo.CoinAddressOMNI) {
          this.getUserCoindAddress();
        } else {
          this.codeUrl = this.coinInfo.CoinAddressOMNI;
          this.setQRCode();
        }
      }
    },
    setQRCode() {
      this.$refs.qrcode.innerHTML = "";
      let qrcode = new QRCode("qrcode", {
        width: 132,
        height: 132,
        text: this.codeUrl, // 二维码地址
        colorDark: "#000",
        colorLight: "#fff",
      });
    },
    onCopy(e) {
      this.$toast(this.$t("recommendFriend.copySeccess"));
    },
    onError(e) {
      this.$toast(this.$t("recommendFriend.copyErr"));
    },
    getUserCommodityAccountsInfo() {
      let data = { UserId: this.MINT_auth.UserId };
      GetUserCommodityAccountsInfo(data).then((r) => {
        console.error(r.data);
        const { Total, CNYTotal, DataList } = r.data.Data;
        // console.log(DataList);
        // let arr = [];
        // let res = [];
        // if (this.isPetty) {
        //   DataList.map((_) => {
        //     _.Balance != 0 ? arr.push(_) : "";
        //   });
        // } else {
        //   arr = DataList;
        // }
        // arr = DataList;
        // if (this.KeyWord) {
        //   arr.map((_) => {
        //     let KeyWord = this.KeyWord.toLowerCase();
        //     let Coin = _.Coin.toLowerCase();

        //     Coin.includes(KeyWord) ? res.push(_) : "";
        //   });
        // } else {
        //   res = arr;
        // }
        // if (arr.length == 0) {
        //   this.isEmpty = true;
        // } else {
        //   this.isEmpty = false;
        // }
        // if (this.loading) {
        //   this.$toast.clear();
        //   this.loading = false;
        // }
        // 这里取USDT
        console.error(DataList[0],"this.coinInfo ");
        this.coinInfo = DataList[0];
        this.getRechargeInfo();
        // this.CNYTotal = CNYTotal;
        // this.Total = Total;
      });
    },
  },
};
</script>
<style lang="less" scoped>
// .recharge_qrcode_wrap_main {
//   background-color: @background-color;
// }
.recharge_qrcode_wrap {
  background-color: @background-color;
//   height: 100vh;
  padding: 60px 30px;
  .content {
    .title {
      color: @font_1_color;
      font-size: 28px;
    }
    ul {
      display: flex;
      height: 100px;
      align-items: center;
      justify-content: space-between;
      position: relative;
      padding: 0 30px;
      width: 100%;
      background-color: #f5f6f7;
      border-radius: 20px;
      &:not(:last-of-type) {
        .bot_br;
      }
      &.chainName {
        background-color: transparent;
        border-bottom: none;
      }
      li {
        height: 80px;
        display: flex;
        align-items: center;
        label {
          display: block;
          font-size: 28px;
          color: @important-font-color;
          margin-right: 20px;
          width: 160px;
          text-align-last: justify;
        }
        &:nth-of-type(1) {
          width: 100%;
          > div {
            display: flex;
            padding: 0;
            align-items: center;
            margin: 0;
            width: 100%;
            height: 100%;

            .main_btn {
              width: 50%;
              height: 80%;
              // width: 120px;
              margin: 0 10px;
              background-color: @panel_color;
              color: @font_2_color;
              cursor: pointer;
              &.active {
                color: #fff;
                background-color: @main2_color;
              }
            }
          }
        }
        input {
          height: 100%;
          width: 100%;
          font-size: 26px;
          background-color: transparent;
        }
        .address {
          width: 100%;
          background-color: transparent;
          resize: none;
        }
        img {
          height: 35px;
        }
        .vcode_bnt {
          position: absolute;
          top: 50%;
          right: 10px;
          padding: 10px;
          // border: 1px solid @main_color;
          transform: translate(0, -50%);
          background: transparent;
          color: @main_color;
          width: 160px;
          border-radius: 8px;
          &:disabled {
            color: @minor-font-color;
            border: 1px solid @minor-font-color;
          }
        }
      }
    }
    .qr {
      display: block;
      margin: 0 auto;
      display: flex;
      justify-content: center;
      margin-top: 100px;
    }
    .qr_tips {
      text-align: center;
      padding: 30px 0;
      width: 50%;
      margin: 0 auto;
      color: @font_f_color;
    }
    .mc {
      padding: 15px;
      background-color: @panel_color;
    }
    .url {
      text-align: center;
      p {
        padding: 20px;
        color: @font_1_color;
        width: 80%;
        margin: 0 auto;
        font-size: 28px;
      }
      span {
        font-size: 28px;
      }
    }
  }
}
</style>