<template>
  <div class="news_wrap">
    <div class="list-wrapper" v-if="!isDetail">
      <van-list
        v-model="loading"
        :finished="finished"
        :finished-text="$t('base.noMore')"
        @load="onLoad"
      >
        <div
          class="notice"
          v-for="(i, index) in information"
          @click="() => toDetail(i)"
          :key="index"
        >
          <div class="notice_right">
            <p class="title">
              <span>{{ i.fullhead }} </span>
            </p>
            <!-- <p class="doc">
            {{i.description}}
          </p> -->
          </div>
          <div class="notice_bot">
            <span :class="{ active: i.isread != 1 }">{{
              $t("base.LearnMore")
            }}</span>
            <img src="../assets/images/common/arrow_right.png" alt="" />
          </div>
        </div>
      </van-list>
    </div>
    <div class="details-wrapper" v-else>
      <div class="back-btn" @click="isDetail = false">
        {{ $t("base.back") }}
      </div>
      <H5Center ref="H5Center"></H5Center>
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
import H5Center from "@/components/H5";
import { GetInformationPageList, UpdateUserEntity } from "@/api";
export default {
  name: "news",
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
    H5Center,
  },
  data() {
    return {
      information: [],
      firstInformation: {},
      PageIndex: 0,
      loading: false,
      finished: false,
      refreshing: false,
      MINT_auth: {},
      isDetail: false,
    };
  },
  mounted() {
    // document.body.style.background = "#EAF0F8";
    let MINT_auth = localStorage.getItem("MINT_auth");
    MINT_auth = MINT_auth ? this.$decryptByDES(MINT_auth) : "";
    this.MINT_auth = MINT_auth ? JSON.parse(MINT_auth) : "";
  },
  methods: {
    getData() {
      let data = {
        PageSize: 20, //每页数</br>
        PageIndex: this.PageIndex, //当前页</br>
        UserId: this.MINT_auth ? this.MINT_auth.UserId : "default", //用户编号(默认传default，若已登录则传用户token)
      };
      GetInformationPageList(data).then((r) => {
        console.log(r.data);
        if (r.data.Code == 200) {
          this.information.push(...r.data.Data.rows);
          this.firstInformation = this.information[0];
          this.loading = false;
        } else {
          this.finished = true;
          if (this.information.length == 0) {
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
        this.getData();
      }, 1000);
    },
    toDetail(i) {
      if (this.MINT_auth && i.isread != 1) {
        let data = {
          UserId: this.MINT_auth.UserId,
          Action: "NewsRead",
          ActionValue: i.newsid,
        };
        const that = this;
        UpdateUserEntity(data).then((r) => {
          if (r.data.Code == 200) {
            let href = this.$config.imgPath + `/News/Detail/${i.newsid}`;
            // this.$router.push({
            //   path: "/H5",
            //   query: {
            //     url: this.$config.imgPath + `/News/Detail/${i.newsid}`,
            //     title: this.$t("news.detailTitle"),
            //   },
            // });
            that.showDetail(href);
          }
        });
      } else {
        // this.$router.push({
        //   path: "/H5",
        //   query: {
        //     url: this.$config.imgPath + `/News/Detail/${i.newsid}`,
        //     title: this.$t("news.detailTitle"),
        //   },
        // });
        let href = this.$config.imgPath + `/News/Detail/${i.newsid}`;
        this.showDetail(href);
      }
    },
    showDetail(url) {
      this.isDetail = true;
      setTimeout(() => {
        this.$refs.H5Center.setIframeUrl(url);
      });
    },
  },
  destroyed() {
    // document.body.style.background = "#fff";
  },
};
</script>
<style lang="less" scoped>
.news_wrap {
//   background-color: @background-color;
  height: 90%;
  .top {
    height: 360px;
    position: relative;

    .pic {
      padding: 20px;
      height: 100%;
      img {
        border-radius: 20px;
        width: 100%;
        height: 100%;
      }
    }
    .txt {
      position: absolute;
      bottom: 30px;
      padding: 20px 20px;
      color: #fff;
      width: 100%;
      font-weight: 600;
    }
  }
  .list-wrapper {
    padding-top: 60px;
  }
  .details-wrapper {
    position: relative;
    padding-top: 60px;
    .back-btn {
      position: absolute;
      top: 20px;
      left: 0px;
      width: 100px;
      height: 40px;
      background: @main_color;
      color: @font_3_color;
      opacity: 0.7;
      border-radius: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      img {
        width: 20px;
        height: 20px;
      }
    }
  }
  .notice {
    margin: 20px;
    padding: 6px;
    background-color: @panel_color;
    border-radius: 24px;
    .notice_bot {
      display: flex;
      justify-content: space-between;
      padding: 10px 20px;
      // border-top: 1px solid #63737A;
      color: @font_1_color;
      align-items: center;
      .active {
        color: @font_f_color;
      }
      img {
        width: 10px;
        height: 20px;
      }
    }
    .notice_right {
      padding: 20px 20px 0 20px;
      // width: 70%;
      // padding-left: 20px;
      .title {
        display: flex;
        justify-content: space-between;
        span {
          &:nth-of-type(1) {
            color: #fff;
            font-weight: 600;
            width: 80%;
            display: block;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            // color: @sub-font-color;
            -webkit-box-orient: vertical;
          }
        }
      }
      .doc {
        margin-top: 10px;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        color: @sub-font-color;
      }
    }
  }
}
</style>