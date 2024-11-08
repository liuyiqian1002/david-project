<template>
  <div class="login_wrap_warpper">
    <div class="form-wrapper">
      <loginBox></loginBox>
    </div>
  </div>
</template>

<script>
import loginBox from "./loginBox.vue";
export default {
  name: "login",
  components: {
    loginBox,
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
    };
  },
  mounted() {
    this.navList = [this.$t("login.navPhone"), this.$t("login.navEmail")];
    this.redirect = this.$route.query.redirect
      ? this.$route.query.redirect
      : "/";
    // this.selPhoneAre();
    // var windowHeight = document.documentElement.clientHeight;
    // this.$refs.login_wrap.style.minHeight = windowHeight + "px";
  },
  methods: {
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
  },
};
</script>
<style lang="less" scoped>
.login_wrap_warpper {
  position: relative;
  height: 100vh;
  overflow: hidden;
  background: url(../assets/pc/蒙版组 6.png) no-repeat;
  background-size: 60% 100%;
  .form-wrapper {
    position: absolute;
    top: 50%;
    left: 66%;
    transform: translate(-30%, -50%);
  }
}
</style>