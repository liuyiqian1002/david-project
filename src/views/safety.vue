<template>
  <div class="safety_wrap_main">
    <!-- <van-nav-bar
      :title="$t('safety.title')"
      left-text
      left-arrow
      @click-left="$router.push('/property')"
    /> -->
    <div class="safety_wrap" v-if="!currentNav.label">
      <div class="contnet">
        <ul @click="currentNav = i" v-for="(i, index) in navList" :key="index">
          <li>
            {{ i.label }}
          </li>
          <li>
            <img src="../assets/images/common/back_right.png" alt="" />
          </li>
        </ul>
      </div>
    </div>
    <div class="details" v-else>
      <div class="back" @click="currentNav = {}"><</div>
      <div class="title">
        <p>{{ currentNav.label }}</p>
      </div>
      <component :is="currentNav.component"></component>
    </div>
  </div>
</template>

<script>
// import QRCode from 'qrcodejs2';
import { Icon, NavBar, IndexBar } from "vant";
import loginPwd from "./loginPwd.vue";
import transactionPwd from "./transactionPwd.vue";
import account from "./account.vue";
import authorization from "./authorization.vue";
export default {
  name: "receivingQRCode",
  components: {
    [Icon.name]: Icon,
    [IndexBar.name]: IndexBar,
    [NavBar.name]: NavBar,
    loginPwd,
    transactionPwd,
    account,
    authorization,
  },
  data() {
    return {
      currentNav: {},
      navList: [
        {
          link: "/login_pwd",
          label: this.$t("safety.upLoginPwd"),
          component: loginPwd,
        },
        {
          link: "/transaction_pwd",
          label: this.$t("safety.transactionPwd"),
          component: transactionPwd,
        },
        {
          link: "/account",
          label: this.$t("safety.AccountManagement"),
          component: account,
        },
        {
          link: "/authorization",
          label: this.$t("safety.codeAuth"),
          component: authorization,
        },
      ],
    };
  },
  mounted() {
    // document.body.style.background='#f5f5f5';
  },
  destroyed() {
    // document.body.style.background='#fff';
  },
};
</script>
<style lang="less" >
.safety_wrap_main {
  background-color: @background-color;
  .van-hairline--bottom {
    &::after {
      content: "";
      border-bottom-width: 0px;
      border: none !important;
    }
  }
  .van-nav-bar {
    background-color: transparent;
  }
}
</style>
<style lang="less" scoped>
.safety_wrap {
  // height: 100vh;
  .contnet {
    margin: 50px 0 0 0;
    ul {
      background-color: @panel_color;
      border-radius: 16px;
      padding: 10px;
      margin-bottom: 20px;
      display: flex;
      // height: 80px;
      align-items: center;
      justify-content: space-between;
      position: relative;
      padding: 20px;
      width: 100%;
      color: @font_1_color;
      &:not(:last-of-type) {
        .bot_br;
      }
      li {
        display: flex;
        align-items: center;
        height: 60px;
        &:nth-of-type(1) {
          width: 70%;
          font-size: 20px;
        }
        &:nth-of-type(2) {
          img {
            height: 25px;
          }
        }
        input {
          height: 100%;
          width: 100%;
          font-size: 26px;
        }
        .vcode_bnt {
          position: absolute;
          top: 50%;
          right: 10px;
          border: 1px solid @main_color;
          transform: translate(0, -50%);
          background: transparent;
          color: @main_color;
          padding: 5px 10px;
          width: 160px;
          &:disabled {
            color: @minor-font-color;
            border: 1px solid @minor-font-color;
          }
        }
      }
    }
  }
  .btns {
    margin-top: 50px;
    padding: 20px;
  }
}
.details {
  position: relative;
  .back{
    position: absolute;
    left: 20px;
    top: 20px;
    font-size: 30px;
    color: @font_3_color;
    cursor: pointer;
  }
  .title {
    font-size: 20px;
    padding: 10px 0;
    p {
      width: 360px;
      color: @font_3_color;
      padding: 10px;
      font-size: 24px;
      margin: 0 auto;
    }
  }
}
</style>