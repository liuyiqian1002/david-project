<template>
  <div class="register_wrap_box">
    <div class="form-wrapper">
      <registerBox></registerBox>
    </div>
  </div>
</template>

<script>
// import {
//   Icon,
//   Image,
//   List,
//   CountDown,
//   Popup,
//   NavBar,
//   IndexBar,
//   IndexAnchor,
//   Cell,
//   Empty,
//   DropdownMenu,
//   Sticky,
//   DropdownItem,
//   Field,
//   Picker,
// } from "vant";
// import { Registered, GetMobileCode, GetMobileAreaCodeList } from "@/api";
// import downLoad from "@/components/downLoad";
// import { IsEmail, isPwd } from "@/libs/util";
// import { Locale } from "vant";
// import enUS from "@/assets/i18n/lang/en-US";
// import jaJP from "@/assets/i18n/lang/ar-sa";
// import zhHK from "@/assets/i18n/lang/es-POR";
// import zhCN from "@/assets/i18n/lang/es-ES";
import registerBox from "./registerBox";
export default {
  name: "forget",
  components: {
    // [Sticky.name]: Sticky,
    // [Icon.name]: Icon,
    // [List.name]: List,
    // [CountDown.name]: CountDown,
    // [Popup.name]: Popup,
    // [NavBar.name]: NavBar,
    // [IndexBar.name]: IndexBar,
    // [IndexAnchor.name]: IndexAnchor,
    // [Cell.name]: Cell,
    // [Empty.name]: Empty,
    // [DropdownMenu.name]: DropdownMenu,
    // [DropdownItem.name]: DropdownItem,
    // [Picker.name]: Picker,
    // [Popup.name]: Popup,
    // downLoad,
    registerBox
  },
  data() {
    return {
      checked: true,
      isReg: true,
      Account: "",
      Password: "",
      Password1: "",
      RecommendEnCode: "", //邀请码（选填）</br>
      Code: "", //验证码<br/>
      isGetCode: true,
      btnTips: "",
      navList: [],
      navIndex: 0,
      showSelPhoneAre: false, //区号选择
      phoneAreList: [], //区号
      PhoneAre: "1",
      Email: "",
      Phone: "",
      isShare: false,
      showDownLoad: false,
    };
  },
  created() {
    // let lang = this.$route.query.lang;
    // if (lang) {
    //   localStorage.setItem("MINT_lang", lang);
    // } else {
    //   lang = localStorage.getItem("MINT_lang")
    //     ? localStorage.getItem("MINT_lang")
    //     : "en";
    //   // localStorage.setItem('MINT_lang','en');
    //   // lang = 'en'
    // }
    // this.$i18n.locale = lang;
    // if (lang == "zh-CN") {
    //   Locale.use("zh-CN", zhCN);
    // } else if (lang == "zh-hk") {
    //   Locale.use("zh-HK", zhHK);
    // } else if (lang == "jpn") {
    //   Locale.use("ja-JP", jaJP);
    // } else {
    //   Locale.use("en-US", enUS);
    // }
  },
  mounted() {
    // this.RecommendEnCode = this.$route.query.inviteCode;

    // if (this.$route.query.inviteCode) {
    //   this.isShare = true;
    // }

    // this.btnTips = this.$t("forgetPwd.btnGetCode");
    // this.selPhoneAre();
    // this.navList = [this.$t("login.navPhone"), this.$t("login.navEmail")];
    // this.showDownLoadPop();
  },
  methods: {
    // onConfirm(value, index) {
    //   this.PhoneAre = value.value;
    //   this.showSelPhoneAre = false;
    // },
    //获取手机区号列表
    // selPhoneAre() {
    //   GetMobileAreaCodeList({}).then((r) => {
    //     console.log(r);
    //     if (r.data.Code == 200) {
    //       let arr = [];
    //       r.data.Data.map((_) => {
    //         arr.push({
    //           value: _.itemvalue,
    //           text: `+${_.itemvalue}   ${_.description}`,
    //         });
    //       });
    //       this.phoneAreList = arr;
    //     }
    //   });
    // },
    // getCode() {
    //   //[WebMethod(Description = @"[88]系统 =>发送验证码(输入参数string Json)</br>
    //   //Mobile:邮箱</br>
    //   //Title:发送类别(用户注册、密码找回、交易密码、转账、提币)</br>
    //   let data = {
    //     Title: "用户注册",
    //     Mobile:
    //       this.navIndex == 1
    //         ? this.Email.trim()
    //         : (this.PhoneAre + "|" + this.Phone).trim(),
    //   };
    //   if (this.navIndex == 1) {
    //     if (!IsEmail(this.Email.trim())) {
    //       this.$toast(this.$t("register.emailErr"));
    //       return;
    //     }
    //   } else {
    //     if (!this.Phone) {
    //       this.$toast(this.$t("login.phone"));
    //       return;
    //     }
    //   }
    //   this.$toast.loading({
    //     duration: 0, // 持续展示 toast
    //     forbidClick: true,
    //   });
    //   this.isGetCode = false;
    //   GetMobileCode(data).then((r) => {
    //     console.log(r);
    //     if (r.data.Code == 200) {
    //       this.$toast(this.$t("register.sendSeccess"));
    //       let wait = 60;
    //       let time = setInterval((_) => {
    //         wait--;
    //         if (wait == 0) {
    //           clearInterval(time);
    //           this.btnTips = this.$t("forgetPwd.btnGetCode");
    //           this.isGetCode = true;
    //         } else {
    //           this.btnTips = wait + "S";
    //         }
    //       }, 1000);
    //     } else {
    //       this.isGetCode = true;
    //     }
    //   });
    // },
    // sign() {
    //   let data = {
    //     RegisterMark: this.navIndex == 1 ? "Email" : "Mobile",
    //     RegisterType: "Web",
    //     Account: this.navIndex == 1 ? this.Email : this.Phone,
    //     Password: this.Password,
    //     OpenId: this.navIndex == 1 ? "Default" : this.PhoneAre, //第三方快捷返回值说明：不是快捷注册传default)</br>
    //     RecommendEnCode: this.RecommendEnCode
    //       ? this.RecommendEnCode
    //       : "Default", //this.RecommendEnCode,//邀请码（选填）</br>
    //     Code: this.Code, //验证码<br/>
    //   };
    //   if (!this.Code) {
    //     this.$toast(this.$t("register.codeErr"));
    //     return;
    //   }
    //   if (this.navIndex == 1) {
    //     if (!IsEmail(this.Email.trim())) {
    //       this.$toast(this.$t("register.emailErr"));
    //       return;
    //     }
    //   }
    //   if (!isPwd(this.Password)) {
    //     this.$toast(this.$t("register.passwordErr"));
    //     return;
    //   }
    //   if (this.Password != this.Password1) {
    //     this.$toast(this.$t("register.passwordErr1"));
    //     return;
    //   }
    //   if (!this.checked) {
    //     this.$toast(this.$t("register.isAgree"));
    //     return;
    //   }
    //   this.isReg = false;
    //   console.log(data);
    //   this.$toast.loading({
    //     duration: 0, // 持续展示 toast
    //     forbidClick: true,
    //   });
    //   Registered(data).then((r) => {
    //     this.isReg = true;
    //     console.log(r);
    //     const { data } = r;

    //     if (data.Code == 200) {
    //       let that = this;
    //       this.$toast({
    //         message: this.$t("register.secces"),
    //         onClose() {
    //           that.$router.push("/login");
    //         },
    //       });
    //     }
    //   });
    // },
    // 清除
    // delTxt(val) {
    //   this[val] = "";
    //   this.$refs[val].focus();
    // },
    // toH5() {
    //   let lang = localStorage.getItem("MINT_lang");
    //   let url =
    //     this.$config.imgPath +
    //     `/News/AppSynopsis/8f14ea47-5fce-4741-aee8-4d85eeb7a69c?LanguageVersion=${lang}`;
    //   this.$router.push({
    //     path: "/H5",
    //     query: {
    //       url: url,
    //       title: this.$t("register.agreement"),
    //     },
    //   });
    // },
    // showDownLoadPop() {
    //   let downloadTime = localStorage.getItem("download");
    //   if (!downloadTime) {
    //     this.showDownLoad = true;
    //   } else {
    //     console.log(Date.now() - Number(downloadTime) > 1000 * 60 * 60 * 12);
    //     this.showDownLoad =
    //       Date.now() - Number(downloadTime) > 1000 * 60 * 60 * 12;
    //   }
    // },
    // closeDownLoad(e) {
    //   this.showDownLoad = false;
    // },
  },
};
</script>
<style lang="less" >
// .register_wrap {
//   .van-nav-bar .van-nav-bar__left .van-icon {
//     color: #fff;
//   }
//   .van-hairline--bottom::after {
//     border-bottom: unset;
//   }
// }
</style>
<style lang="less" scoped>
.register_wrap_box {
  position: relative;
  height: 100vh;
  overflow: hidden;
  background: url(../assets/pc/蒙版组 9.png) no-repeat;
  background-size: 60% 100%;
  .form-wrapper {
    position: absolute;
    top: 50%;
    left: 66%;
    transform: translate(-30%, -50%);
  }
}
</style>