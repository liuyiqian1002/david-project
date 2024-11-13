<template>
  <div class="property_wrap">
    <sheader></sheader>

    <div class="content-wrapper">
      <div class="nav-list">
        <div class="me_menu">
          <div class="content">
            <div
              class="cell"
              :class="{ active: activeIndex == 0 }"
              @click="$router.push('/property') && (activeIndex = 0)"
            >
              <div class="cion">
                <img src="../assets/pc/路径 6@3x.png" alt="" />
              </div>
              <div>
                <span>{{ $t("mine.personalInfo") }}</span>
              </div>
            </div>
            <!-- news -->
            <div class="cell" :class="{ active: activeIndex == 7 }" @click="activeIndex = 7">
              <div class="pic">
                <img src="../assets/pc/路径 14@3x.png" alt="" />
              </div>
              <div>
                <p>{{ $t("mine.news") }}</p>
              </div>
            </div>
            <!-- transaction -->
            <div class="cell" :class="{ active: activeIndex == 6 }" @click="activeIndex = 6">
              <div class="cion">
                <img src="../assets/pc/组 4@3x.png" alt="" />
              </div>
              <div>
                <span>{{ $t("mine.pay") }}</span>
              </div>
            </div>
            <!-- recharge -->
            <div class="cell" :class="{ active: activeIndex == 4 }" @click="activeIndex = 4">
              <div class="cion">
                <img src="../assets/images/common/cjwt.png" alt="" />
              </div>
              <div>
                <span>{{ $t("property.recharge") }}</span>
              </div>
            </div>
            <!-- withdrawal -->
            <div class="cell" :class="{ active: activeIndex == 5 }" @click="activeIndex = 5">
              <div class="cion">
                <img src="../assets/images/common/cjwt.png" alt="" />
              </div>
              <div>
                <span>{{ $t("property.withdraw") }}</span>
              </div>
            </div>

            <!-- $router.push('/recommend_friend') &&  -->
            <div class="cell" :class="{ active: activeIndex == 1 }" @click="activeIndex = 1">
              <div class="cion">
                <img src="../assets/images/common/swhz.png" alt="" />
              </div>
              <div>
                <span>{{ $t("mine.inviteFriends") }}</span>
              </div>
            </div>
            <!-- $router.push('/safety') -->
            <div class="cell" :class="{ active: activeIndex == 2 }" @click="activeIndex = 2">
              <div class="cion">
                <img src="../assets/images/common/ic_anquan.png" alt="" />
              </div>
              <div>
                <span>{{ $t("mine.securityCenter") }}</span>
              </div>
            </div>
            <!-- 吧 -->
            <div class="cell" :class="{ active: activeIndex == 3 }" @click="toService">
              <div class="cion">
                <img src="../assets/images/common/gywm.png" alt="" />
              </div>
              <div>
                <span>{{ $t("mine.help") }}</span>
              </div>
            </div>

            <div class="cell" :class="{ active: activeIndex == 8 }" @click="outSign">
              <div class="cion">
                <img src="../assets/images/common/out_login.png" alt="" />
              </div>
              <div>
                <span>{{ $t("mine.out") }}</span>
                <!-- <img src="../assets/images/common/back_right.png" alt="" /> -->
              </div>
            </div>
          </div>
          <!-- <div class="my_code" @click="toQrCode">
            <div>
              <div class="pic">
                <img src="../assets/images/common/me_code.png" alt="" />
              </div>
              <p>{{ $t("mine.QrCode") }}</p>
            </div>
          </div> -->
        </div>
      </div>
      <div class="content-box">
        <div v-if="activeIndex == 0">
          <userInfo></userInfo>
        </div>
        <div v-else-if="activeIndex == 1">
          <recommendFriend></recommendFriend>
        </div>
        <div v-else-if="activeIndex == 2">
          <safety></safety>
        </div>
        <div v-else-if="activeIndex == 5">
          <withdrawCoin></withdrawCoin>
        </div>
        <div v-else-if="activeIndex == 6">
          <transaction></transaction>
        </div>
        <div v-else-if="activeIndex == 4">
          <rechargeQrcode ref="rechargeQrcode"></rechargeQrcode>
        </div>
        <div v-else-if="activeIndex == 3">
          <H5Center ref="H5HelpCenter"></H5Center>
        </div>
        <div v-else-if="activeIndex == 7">
          <news></news>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
// import mainFooter from "@/components/mainFooter";
import userInfo from "@/views/user/userInfo";
import safety from "@/views/safety";
import H5Center from "@/components/H5";
import recommendFriend from "@/views/recommendFriend";
// import recharge from "./recharge.vue";
import rechargeQrcode from "./rechargeQrcode.vue";
import accRecord from "./accRecord.vue";
import withdrawCoin from "./withdrawCoin.vue";
import transaction from "./transaction.vue";
import news from "./News.vue";
import { GetUserCommodityAccountsInfo, GetEntityUser, SignOut } from "@/api";
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
  Sticky,
  Dialog,
  List,
  CountDown,
} from "vant";
// import AccRecord from './accRecord.vue';
export default {
  name: "property",
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
    [Dialog.name]: Dialog,
    [CountDown.name]: CountDown,
    // mainFooter,
    userInfo,
    safety,
    recommendFriend,
    H5Center,
    // recharge,
    withdrawCoin,
    transaction,
    news,
    rechargeQrcode,
    accRecord,
  },
  data() {
    return {
      MINT_auth: {},
      CNYTotal: "",
      DataList: [],
      Total: "",
      isPetty: false, //小额
      emptyImg: "",
      isEmpty: false,
      KeyWord: "",
      showMeMenu: false,
      avatar: "",
      UserInfo: {},
      loading: false,
      activeIndex: 0,
      helpCenterUrl: "",
    };
  },

  mounted() {
    this.emptyImg = require("../assets/images/common/empty_img.png");
    console.log(this.$route.query);
    const { index } = this.$route.query;
    if (index !== undefined) {
      this.activeIndex = index;
    }
    let MINT_auth = localStorage.getItem("MINT_auth");
    MINT_auth = this.$decryptByDES(MINT_auth);
    this.MINT_auth = JSON.parse(MINT_auth);
    this.getUserCommodityAccountsInfo();
    //   history.replaceState('state', 'title', '.#/transaction')
    this.getEntityUser();
  },
  methods: {
    search() {
      this.loading = true;
      this.$toast.loading({
        duration: 0, // 持续展示 toast
        forbidClick: true,
      });
      this.getUserCommodityAccountsInfo();
    },
    getUserCommodityAccountsInfo() {
      let data = { UserId: this.MINT_auth.UserId };
      GetUserCommodityAccountsInfo(data).then((r) => {
        console.log(r.data);
        const { Total, CNYTotal, DataList } = r.data.Data;
        console.log(DataList);
        let arr = [];
        let res = [];
        if (this.isPetty) {
          DataList.map((_) => {
            _.Balance != 0 ? arr.push(_) : "";
          });
        } else {
          arr = DataList;
        }
        if (this.KeyWord) {
          arr.map((_) => {
            let KeyWord = this.KeyWord.toLowerCase();
            let Coin = _.Coin.toLowerCase();

            Coin.includes(KeyWord) ? res.push(_) : "";
          });
        } else {
          res = arr;
        }
        if (arr.length == 0) {
          this.isEmpty = true;
        } else {
          this.isEmpty = false;
        }
        if (this.loading) {
          this.$toast.clear();
          this.loading = false;
        }
        this.DataList = res;
        console.log(this.DataList);
        this.CNYTotal = CNYTotal;
        this.Total = Total;
        // this.toRecord(this.DataList[0]);
      });
    },
    petty() {
      this.isPetty = !this.isPetty;
      this.loading = true;
      this.$toast.loading({
        duration: 0, // 持续展示 toast
        forbidClick: true,
      });
      this.getUserCommodityAccountsInfo();
    },
    toRecord(i) {
      this.$store.commit("setCoinInfo", i);
      // this.$nextTick((_) => {
      //   console.error(this.$refs.accRecord,'accRecord');
      //   this.$refs.accRecord.setCoinInfo(i);
      // })
      // this.$router.push({
      //   path: "/acc_record",
      //   query: {
      //     coin: i.Coin,
      //   },
      // });
    },
    getEntityUser() {
      let data = {
        UserId: this.MINT_auth.UserId,
      };
      GetEntityUser(data).then((r) => {
        // console.log(JSON.stringify(r.data.Data) );
        if (r.data.Code == 200) {
          this.UserInfo = r.data.Data;
          const { UserGrade } = r.data.Data;
          console.log(this.UserInfo);
          this.$store.commit("setUserInfo", this.UserInfo);
          this.UserInfo.WithdrawPassword == ""
            ? (this.hasPayPwd = false)
            : (this.hasPayPwd = true);
          let num = UserGrade + 1;
          this.avatar = r.data.Data.HeadIcon
            ? this.$config.imgPath + r.data.Data.HeadIcon
            : require("../assets/images/common/logo.png");
          this.UserGradeFilePath =
            this.$config.imgPath + r.data.Data.UserGradeFilePath; //require(`../assets/images/common/vip/vip${num}@2x.png`);
        }
      });
    },
    pay() {
      //   let hasPayPwd = true; // 是否设置交易密码
      if (!this.hasPayPwd) {
        this.$router.push("/transaction_pwd");
      } else {
        this.$router.push("/transaction");
      }
    },
    outSign() {
      // //region 用户 => 退出登录
      this.showMeMenu = false;
      let data = {
        UserId: this.UserInfo.UserId,
      };
      Dialog.confirm({
        title: this.$t("mine.outTips"),
        confirmButtonText: this.$t("base.confirm"),
        cancelButtonText: this.$t("base.cancel"),
      })
        .then(() => {
          SignOut(data).then((r) => {
            let that = this;
            if (r.data.Code == 200) {
              localStorage.removeItem("MINT_auth");
              localStorage.removeItem("isRead");
              localStorage.removeItem("userInfo");
              localStorage.removeItem("isBindPhone");
              this.$toast({
                message: that.$t("mine.seccess"),
                onClose() {
                  that.$router.replace("/");
                },
              });
            }
          });
        })
        .catch(() => {
          // on cancel
        });
    },
    toQrCode() {
      this.$router.push({
        path: "/receiving_qrcode",
        query: {
          code: this.UserInfo.EnCode,
        },
      });
    },
    toService() {
      // this.showMeMenu = false;
      let lang = localStorage.getItem("MINT_lang")
        ? localStorage.getItem("MINT_lang")
        : "zh-CN";
      let href =
        this.$config.imgPath +
        `/News/AppReplicate/c8ebc9cc-8d27-46a5-a57e-c54d1287c4d4?LanguageVersion=${lang}`;
      this.helpCenterUrl = href;
      this.activeIndex = 3;
      console.log(this.$refs.H5HelpCenter, "this.$refs.H5HelpCenter");
      // this.$refs.H5HelpCenter.setIframeUrl(href);
      setTimeout(() => {
        this.$refs.H5HelpCenter.setIframeUrl(href);
      });
      // this.$router.push({
      //   path: "/h5",
      //   query: {
      //     url: href,
      //     title: this.$t("mine.help"),
      //   },
      // });
    },
  },
  computed: {
    ...mapState(["base"]),
  },
  watch: {
    $route(to, from) {
      // console.log(to, from, this.$route.query);
      const { index } = this.$route.query;
      this.activeIndex = index || 0;
    },
  },
};
</script>
<style lang="less" >
.property_wrap {
  // background-color: @background-color;
  color: @font_1_color;

  .custom-image .van-empty__image {
    width: 480px;
    height: 298px;
    margin-top: unset;
  }
}
</style>
<style lang="less" scoped>
.property_wrap {
  .content-wrapper {
    display: flex;
    justify-content: space-around;
    align-items: flex-start;
    .nav-list {
      //
      width: 360px;
      .content {
        width: 100%;
        margin-top: 30px;
        padding-left: 60px;
        .cell {
          display: flex;
          align-items: center;
          height: 68px;
          font-size: 24px;
          font-weight: 500;
          color: #989898;
          cursor: pointer;
          img {
            width: 30px;
            height: 30px;
            margin-right: 20px;
          }
        }
        .active{
          color: @main_color;
        }
      }
    }
    .content-box {
      width: 1000px;
    }
  }
  .me_menu {
    // padding: 20px;
    height: 100%;
    position: relative;
    // display: flex;
    // justify-content: flex-start;
    .my_code {
      position: absolute;
      bottom: 60px;
      left: 42%;
      display: flex;
      justify-content: center;
      > div {
        // width: 100px;
        text-align: center;
        .pic {
          width: 180px;
          height: 80px;
          text-align: center;
          margin: 0 auto;
          img {
            margin: 0 auto;
            display: block;
            width: 100px;
            height: 100px;
          }
        }
      }
    }
    .avatar {
      display: flex;
      align-items: center;
      flex-direction: column;
      padding: 20px 0;
      border-bottom: 1px solid #555;
      .pic {
        width: 120px;
        height: 120px;
        border-radius: 50%;
        overflow: hidden;
        img {
          width: 100%;
          height: 100%;
          transform: scale(0.8);
        }
        margin-right: 10px;
        & + div {
          p {
            &:nth-of-type(1) {
              /*color: #272E3F;*/
              font-weight: 600;
              .tag {
                width: 60px;
                height: 30px;
                font-size: 20px;
                display: inline-block;
                background: url(../assets/images/common/me_tag_bg.png) no-repeat;
                background-size: cover;
                text-align: center;
                color: #916517;
                line-height: 30px;
              }
            }
          }
        }
      }
    }
  }
}
</style>