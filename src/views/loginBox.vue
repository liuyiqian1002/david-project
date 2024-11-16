<template>
  <div class="login_wrap_warpper">
    <!-- <van-nav-bar
      :title="$t('login.title')"
      left-arrow
      left-text
      :right-text="$t('login.rightTitle')"
      @click-right="$router.push('/register')"
      @click-left="$router.replace('/')"
    /> -->
    <div class="login_wrap" ref="login_wrap">
      <!-- <div class="logo"> -->
      <!-- <p class="reg">
        <img
          @click="$router.push('/')"
          src="../assets/images/common/Nav_close.png"
          alt=""
        />
        <span @click="$router.push('/register')">{{
          $t("login.Registernow")
        }}</span>
      </p> -->
      <!-- <div class="pic">
          <img src="../assets/images/common/logo1.png" alt="" />
        </div> -->
      <!-- <p class="welcome">{{ $t("base.welcome") }}</p> -->
      <!-- </div> -->
      <div class="nav" v-show="navIndex == 0">
        <div @click="navIndex = 0" :class="{ active: navIndex == 0 }">
          {{ navList[0] }}
        </div>
        <div @click="navIndex = 1" :class="{ active: navIndex == 1 }">
          {{ navList[1] }}
        </div>
      </div>
      <div class="nav" v-show="navIndex == 1">
        <div @click="navIndex = 1" :class="{ active: navIndex == 1 }">
          {{ navList[1] }}
        </div>
        <div @click="navIndex = 0" :class="{ active: navIndex == 0 }">
          {{ navList[0] }}
        </div>
      </div>
      <!-- <div class="nav">
          <div @click="navIndex = index" :class="{'active':navIndex==index}"  v-for="(i ,index ) in navList">
              {{i}}
          </div>
      </div> -->
      <div class="txt_box">
        <div class="item phone" v-show="navIndex == 0">
          <span @click="showSelPhoneAre = true">+{{ PhoneAre }}</span>
          <input
            type="text"
            :placeholder="$t('login.phone')"
            v-model="Phone"
            ref="Phone"
          /><b v-show="Phone" @click="delTxt('Phone')"></b>
        </div>
        <div class="item" v-show="navIndex == 1">
          <input
            type="text"
            :placeholder="$t('login.email')"
            v-model="Email"
            ref="Email"
          /><b v-show="Email" @click="delTxt('Email')"></b>
        </div>
        <div class="item">
          <input
            type="password"
            :placeholder="$t('login.password')"
            v-model="Password"
            ref="Password"
          /><b v-show="Password" @click="delTxt('Password')"></b>
        </div>
      </div>

      <div class="bot">
        <div class="btns">
          <button
            :disabled="!(Password && (navIndex == 1 ? Email : Phone))"
            class="main_btn"
            @click="login"
          >
            {{ $t("login.btnLogin") }}
          </button>
        </div>
        <div class="nav">
          <div class="lang-box">
            <span @click="showLanguage = true">{{
              $t("login.changeLan")
            }}</span>
            <div
              class="language-box"
              v-if="showLanguage"
              @mouseleave="showLanguage = false"
            >
              <setLang @getBars="getBars"></setLang>
            </div>
          </div>

          <span @click="$router.push('/forget_pwd')">{{
            $t("login.forgetPwd")
          }}</span>
        </div>
        <div class="got-to-register">
          <div class="content-box">
            <span>{{ $t("login.hasAcc") }}？</span>
            <span @click="$router.push('/register')" class="mc">{{
              $t("login.Registernow")
            }}</span>
          </div>
        </div>
      </div>
      <!-- <div class="tips"> -->
      <!-- <p>{{$t('login.hasAcc')}}？ <span class="mc"  @click="$router.push('/register')" >{{$t("login.Registernow")}}</span></p> -->
      <!-- <p class="safety_tips">
          <img src="../assets/images/common/safety_tips.png" alt="" />
          {{ $t("base.safetyTips") }}
        </p> -->
      <!-- </div> -->
      <van-popup v-model="showSelPhoneAre" position="bottom" round>
        <van-picker
          show-toolbar
          :columns="phoneAreList"
          @cancel="showSelPhoneAre = false"
          @confirm="onConfirm"
        />
      </van-popup>
    </div>
  </div>
</template>

<script>
import {
  Icon,
  Image,
  List,
  CountDown,
  Popup,
  NavBar,
  IndexBar,
  IndexAnchor,
  Cell,
  Empty,
  DropdownMenu,
  Sticky,
  DropdownItem,
  Field,
  Picker,
} from "vant";
import { Login, GetEntityUser, GetMobileAreaCodeList } from "@/api";
import { IsEmail, isPwd, encryptByDES } from "@/libs/util";
import setLang from "@/views/setLan";
export default {
  name: "login",
  components: {
    [Sticky.name]: Sticky,
    [Icon.name]: Icon,
    [List.name]: List,
    [CountDown.name]: CountDown,
    [Popup.name]: Popup,
    [NavBar.name]: NavBar,
    [IndexBar.name]: IndexBar,
    [IndexAnchor.name]: IndexAnchor,
    [Cell.name]: Cell,
    [Empty.name]: Empty,
    [DropdownMenu.name]: DropdownMenu,
    [DropdownItem.name]: DropdownItem,
    [Field.name]: Field,
    [Picker.name]: Picker,
    [Popup.name]: Popup,
    setLang,
  },
  data() {
    return {
      Email: "",
      Phone: "",
      Password: "",
      redirect: "",
      navList: [],
      navIndex: 0,
      showSelPhoneAre: false, //区号选择
      phoneAreList: [], //区号
      PhoneAre: "1",
      showLanguage: false,
    };
  },
  mounted() {
    this.navList = [this.$t("login.navPhone"), this.$t("login.navEmail")];
    this.redirect = this.$route.query.redirect
      ? this.$route.query.redirect
      : "/";
    this.selPhoneAre();
    var windowHeight = document.documentElement.clientHeight;
    // this.$refs.login_wrap.style.minHeight = windowHeight + "px";
  },
  methods: {
    login() {
      let data = {
        LoginMark: this.navIndex == 1 ? "Email" : "Mobile",
        Account:
          this.navIndex == 1
            ? this.Email.trim()
            : (this.PhoneAre + "|" + this.Phone).trim(),
        Password: this.Password,
      };
      if (this.navIndex == 1) {
        if (!IsEmail(this.Email.trim())) {
          this.$toast(this.$t("login.emailErr"));
          return;
        }
      }
      if (!isPwd(this.Password)) {
        this.$toast(this.$t("login.passwordErr"));
        return;
      }
      this.$toast.loading({
        duration: 0, // 持续展示 toast
        forbidClick: true,
        message: this.$t("login.loading"),
      });
      Login(data).then((r) => {
        console.log(r);
        const { data } = r;
        if (data.Code == 200) {
          let that = this;
          that.getEntityUser(data.Data.UserId).then((_) => {
            this.$toast({
              duration: 1500,
              message: that.$t("login.secces"),
              onClose() {
                let redirect = that.redirect;
                that.$router.push(redirect);
              },
            });
          });
        }
      });
    },
    // 清除
    delTxt(val) {
      this[val] = "";
      this.$refs[val].focus();
    },
    onConfirm(value, index) {
      this.PhoneAre = value.value;
      this.showSelPhoneAre = false;
    },
    //获取手机区号列表
    selPhoneAre() {
      GetMobileAreaCodeList({}).then((r) => {
        console.log(r);
        if (r.data.Code == 200) {
          let arr = [];
          r.data.Data.map((_) => {
            arr.push({
              value: _.itemvalue,
              text: `+${_.itemvalue}   ${_.description}`,
            });
          });
          this.phoneAreList = arr;
        }
      });
    },
    async getEntityUser(UserId) {
      let data = {
        UserId,
      };
      let resBody = await GetEntityUser(data).then((r) => {
        // console.log(JSON.stringify(r.data.Data) );
        if (r.data.Code == 200) {
          this.UserInfo = r.data.Data;
          const { UserGrade } = r.data.Data;
          console.log(this.UserInfo.UserId);
          let MINT_auth = encryptByDES(JSON.stringify(this.UserInfo));
          localStorage.setItem("MINT_auth", MINT_auth);
          localStorage.setItem("setUserInfo", MINT_auth);
          this.$store.commit("setUserInfo", this.UserInfo);
        }
      });
    },
  },
};
</script>
<style lang="less" scoped>
.login_wrap_warpper {
  position: relative;
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  .login_wrap {
    // background: @background-color;
    position: relative;
    width: 601px;
    height: 600px !important;
    background: #1f1f2f;
    border-radius: 20px;
    opacity: 1;

    height: calc(100vh - 46px);
    color: @font_color;
    padding: 0 48px;
    .logo {
      margin: 0 auto 0;
      text-align: left;
      text-align: center;
      padding-top: 80px;
      .reg {
        display: flex;
        justify-content: flex-end;
        img {
          width: 40px;
          height: 40px;
          position: absolute;
          left: 40px;
        }
      }
      .pic {
        img {
          border-radius: 20px;
          width: 158px;
        }
        padding-bottom: 40px;
      }
      .welcome {
        padding-top: 126px;
        // color: #303030;
        font-size: 46px;
        font-family: PingFang SC;
        font-weight: 500;
      }
    }
    > .nav {
      // width: 80%;
      position: relative;
      margin: 60px auto 0;
      display: flex;
      justify-content: space-between;
      > div {
        color: @font_3_color;
        font-size: 24px;
        padding: 10px 0;
        &.active {
          .cf;
          color: @main_color;
        }
      }
    }
    .got-to-register {
      position: absolute;
      top: 500px;
      width: 90%;
      .content-box {
        width: 350px;
        margin: 0 auto;
        span:nth-of-type(2) {
          cursor: pointer;
        }
      }
    }
    .txt_box {
      // width: 80%;
      margin: 50px auto 0;
      .item {
        margin: 30px 0 0;
        height: 80px;
        line-height: 80px;
        position: relative;
        background-color: @input-background-color;
        border-radius: 8px;
        background-size: contain;
        b {
          height: 80px;
          width: 70px;
          display: block;
          position: absolute;
          right: 0;
          top: 0;
          background: url(../assets/images/common/ic_edit_delete.png) no-repeat
            center;
          background-size: 40px 40px;
        }
        input {
          width: 100%;
          height: 50px;
          padding-left: 30px;
          font-size: 20px;
          background-color: transparent;
          color: @font_3_color;
          &::-ms-input-placeholder {
            /* Internet Explorer 10-11 */
            .input-placeholder;
          }
          &::-webkit-input-placeholder {
            .input-placeholder;
          }
          &::-moz-placeholder {
            /* Mozilla Firefox 19+ */
            .input-placeholder;
          }
          &::-moz-placeholder {
            /* Mozilla Firefox 4 to 18 */
            .input-placeholder;
          }
        }
        &.email {
          input {
            background-size: 30px;
          }
        }
        &.phone {
          display: flex;
          align-items: center;
          span {
            position: relative;
            width: 130px;
            text-align: center;
            margin-right: 10px;
            color: @font_color;
            font-size: 20px;
            &::before {
              content: "";
              width: 20px;
              height: 15px;
              background-size: contain;
              // border-left: 10px solid transparent;
              // border-right: 10px solid transparent;
              // border-top: 10px solid #fff;
              position: absolute;
              top: 0;
              right: 0;
              bottom: 0;
              margin: auto;
            }
          }
        }
        &.pwd {
          input {
            background-color: @panel_color;
            background-size: 30px;
            background-position: 0 8px;
          }
        }
      }
    }
    .bot {
      // width: 80%;
      margin: 20px auto;
      .main_btn {
        height: 60px;
        font-size: 20px;
        background: @btn-background-linear;
        color: @font_3_color;
        font-weight: 600;
        border-radius: 18px;
      }
      .nav {
        margin: 20px 0;
        justify-content: space-between;
        display: flex;
        color: @btn_main_color;
        span {
          text-decoration: underline;
          cursor: pointer;
        }
        div:nth-of-type(1) {
          position: relative;
        }
        .language-box {
          height: 200px;
          width: 220px;
          position: absolute;
          top: 10px;
          left: -20px;
          z-index: 100;
        }
      }
    }
    .tips {
      width: 80%;
      margin: 80px auto;
      font-size: 29px;
      position: absolute;
      //   bottom: 90px;
      left: 0;
      right: 0;
      width: 80%;
      margin: 0px auto;
      span {
        font-size: 29px;
        font-weight: 500;
      }
      .safety_tips {
        // display: flex;
        // align-items: center;
        font-size: 24px;
        margin-top: 15px;
        color: #919191;
        img {
          width: 20px;
          margin-right: 5px;
        }
      }
    }
  }
  .van-nav-bar__text {
    color: @font_3_color;
  }
}
</style>