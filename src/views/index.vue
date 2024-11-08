<template>
  <div class="index_wrap">
    <!-- <downLoad  @closeDownLoad='closeDownLoad' v-show="showDownLoad"/> -->
    <sheader></sheader>
    <div class="content">
      <div class="first_wrap">
        <div class="left">
          <div class="title1">{{ $t("base.joinNum") }}</div>
          <div class="title2">{{ $t("base.explore") }}</div>
          <div class="try">
            <div class="input">
              <input type="text" :placeholder="$t('base.QuicklyInput')" />
            </div>
            <div class="btn">{{ $t("base.try") }}</div>
          </div>
          <p class="text-title">{{ $t("base.DownloadApp") }}</p>
          <div class="download-app">
            <div class="google" @click="downloadApp(1)">
              <img src="../assets/pc/组 3@3x.png" alt="" />
              <p>Google Play</p>
            </div>
            <div class="apple" @click="downloadApp(2)">
              <img src="../assets/pc/路径 5@3x.png" alt="" />
              <p>Apple Store</p>
            </div>
          </div>
        </div>
        <div class="right">
          <img src="../assets/pc/组 1.png" alt="" />
        </div>
      </div>

      <div class="swiper_wrap">
        <div class="swiper-container">
          <div class="swiper-wrapper picslide">
            <div
              class="swiper-slide index_slide"
              v-for="i in banner"
              :key="i.ImageFilePath"
            >
              <a :href="i.ObjectValue">
                <img :src="$config.imgPath + i.ImageFilePath" alt="" />
                <!-- <img src='../assets/images/banner.png' alt=""> -->
              </a>
            </div>
          </div>
          <!-- 分页器 -->
          <div class="swiper-pagination"></div>
        </div>
      </div>

      <div class="notice">
        <div class="notice_right">
          <div class="notice_ico">
            <img src="../assets/pc/路径 14@3x.png" alt="" />
          </div>
          <div>
            <!-- <p class="title">
              <span
                >{{ $t("base.News") }}
              </span>
              <img
                v-show="information.isread != 1"
                src="../assets/images/common/new.png"
                alt=""
              />
            </p> -->
            <p class="doc">
              <!-- {{information.description}} -->
              {{ information.fullhead }}
            </p>
          </div>
        </div>
        <div class="notice_left" @click.stop="newsMore">
          <p>show more</p>
        </div>
      </div>
      <!-- <a :href="QuickCoinURL" ref="QuickCoinURL" @click.stop="toQuick" :target="QuickCoinURL!='#/login'?'_blank':''"> -->

      <div class="description1">{{ $t("base.description1") }}</div>
      <div class="nav-box">
        <div class="nav">
          <div class="left">
            <img src="../assets/pc/组 6198@3x.png" alt="" />
          </div>
          <div class="center">
            <ul>
              <li
                v-for="(i, index) in navList"
                @click="toNavDetail(i)"
                :key="index"
              >
                <div :class="['pic', { hot: i.hot }]">
                  <img :src="i.ico" alt="" />
                </div>
                <p>{{ i.label }}</p>
              </li>
            </ul>
          </div>
          <div class="right">
            <img src="../assets/pc/组 6198@3x.png" alt="" />
          </div>
        </div>
      </div>

      <div class="list-wrapper">
        <div class="left-arrow" @click="goPage(1)">
          <img src="../assets/pc/组 11748@3x.png" alt="" />
        </div>
        <div class="list-box-wrapper">
          <div class="list-box">
            <div
              class="list-item"
              v-for="(vCoins, index) in vCoinsList"
              :key="vCoins.id || index"
            >
              <div class="content">
                <div class="icon"><img :src="vCoins.ico" alt="" /></div>
                <div class="name">{{ `${vCoins.QuoteCurrency} / ${vCoins.BaseCurrency}` }}</div>
                <div class="num">{{ vCoins.Amount }}</div>
                <div class="percent">{{ vCoins.GainRate }}</div>
              </div>
            </div>
          </div>
        </div>
        <div class="right-arrow" @click="goPage(2)">
          <img src="../assets/pc/组 11749@3x.png" alt="" />
        </div>
      </div>

      <!-- <div class="quick_coin" @click="toBuyCoins">
        <div>
          <div class="pic">
            <img src="../assets/images/home/组 6176@2x.png" alt="" />
          </div>
          <div>
            <p>{{ $t("base.Quickly") }}</p>
            <p>{{ $t("base.QuicklySub") }}</p>
          </div>
        </div>
        <div>
          <img src="../assets/images/home/路径 6829.png" alt="" />
        </div>
      </div> -->

      <div class="description2">{{ $t("base.investment") }}</div>
      <div class="description3">
        {{ $t("base.description2") }}
      </div>
      <!-- </a> -->
      <div class="con_list">
        <div class="con_title">
          <div class="lf">
            <img src="../assets/images/home/路径 6831.png" alt="" />
            {{ $t("base.browsing") }}
          </div>
          <div class="fr" @click="$router.push('/market')">
            {{ $t("base.more") }} >
          </div>
        </div>
        <ul
          v-for="(i, index) in SymbolsList"
          @click="toTrad(i)"
          v-if="index < 10"
          :key="index"
        >
          <li>
            <div>
              <p>
                {{ i.BaseCurrency }}<span>/{{ i.QuoteCurrency }}</span>
              </p>
            </div>
          </li>
          <li>
            <div>
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
              :class="[{ down: i.RisingValue < 0 }, { up: i.RisingValue >= 0 }]"
            >
              <img :src="require(`../assets/pc/${i.RisingValue >= 0 ? '路径 17' : '路径 19159'}.png`)" /><p>{{ gainRate(i) }}%</p>
            </div>
          </li>
        </ul>
      </div>

      <div class="description2">{{ $t("base.reliable") }}</div>
      <div class="description3">
        {{ $t("base.safety") }}
      </div>
      <!-- <div  class="service" >
        <img src="../assets/images/common//ic_contract_service_small1.png" alt="">
      </div> -->
    </div>
    <div class="second-wrapper">
      <div class="content">
        <div class="item" v-for="(item, index) in secondList" :key="index">
          <div class="title">{{ item.title }}</div>
          <div class="desc">{{ item.desc }}</div>
        </div>
      </div>
    </div>
    <van-dialog
      v-model="showNotice"
      :style="{ top: '50%' }"
      class="notice_wrap"
      :showConfirmButton="false"
    >
      <div class="notice_ifram">
        <iframe
          ref="NoticeIfram"
          :src="NoticeIframURL"
          frameborder="0"
        ></iframe>
      </div>
      <div class="btns">
        <button @click="confirmNotice">{{ $t("base.confirm") }}</button>
      </div>
    </van-dialog>
    <search @changeSt="closrSearch" Flag="2" @search="search" :nShow="nShow" />
    <sfooter />
  </div>
</template>

<script>
var dx, dy;
var screenWidth = window.screen.width;
var screenHeight = window.screen.height;
import { Icon, NavBar, IndexBar, Tab, Tabs, Popup, List, Dialog } from "vant";
import {
  GetAppVersion, //测试
  GetRotateImageList, //轮播
  GetInformationPageList,
  GetSymbolsPageList,
  GetQuicklyBuyCoinsUrl,
  GetLatestNotice,
  UpdateUserEntity,
} from "@/api";
import { decryptByDES } from "@/libs/util";
import Swiper from "swiper";
import search from "@/components/search";
import config from "@/config";
// import mainFooter from "@/components/mainFooter";
import sheader from "@/components/sheader";
import sfooter from "@/components/sfooter";
export default {
  name: "Home",
  components: {
    sfooter,
    sheader,
    search,
    [NavBar.name]: NavBar,
    [Icon.name]: Icon,
    [Dialog.name]: Dialog,
  },
  data() {
    return {
      lang: "",
      banner: [
        {
          ImageFilePath: "",
        },
      ],
      information: { filepath: "", isread: 1 },
      SymbolsList: [{}],
      Time: "",
      flags: false,
      MINT_auth: {},
      position: {
        x: screenWidth - 50,
        y: "80%",
      },
      nShow: false,
      // showDownLoad:true,
      navList: [],
      QuickCoinURL: "#/login",
      NoticeIframURL: "",
      showNotice: false,
      NoticeId: "",
      vCoinsList: [],
      secondList: [],
      transformWidth: 0,
    };
  },
  mounted() {
    console.error(window.screen.width, window.screen.height);
    this.navList = [
      {
        ico: require("../assets/pc/组 6158.png"),
        url: "/mining",
        label: this.$t("base.indexNav"),
        hot: false,
      },
      {
        ico: require("../assets/pc/组 6159.png"),
        url: "/blind_box",
        label: this.$t("base.indexNav1"),
        hot: false,
      },
      {
        ico: require("../assets/pc/组 6160.png"),
        url: "/guess",
        label: this.$t("base.indexNav2"),
        hot: false,
      },
      {
        ico: require("../assets/pc/组 6161.png"),
        url: "/financial",
        label: this.$t("base.indexNav3"),
      },
    ];
    // document.body.style.backgroundColor='#f6f6f6'
    this.lang = localStorage.getItem("MINT_lang");
    let MINT_auth = localStorage.getItem("MINT_auth");
    MINT_auth = MINT_auth ? this.$decryptByDES(MINT_auth) : "";
    this.MINT_auth = MINT_auth ? JSON.parse(MINT_auth) : "";
    console.log(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>");
    console.log(this.MINT_auth);
    if (this.MINT_auth) {
      this.toQuickCoin();
      this.getLatestNotice().then((_) => {
        if (!this.MINT_auth.Mobile && !this.showNotice) {
          let readTime = localStorage.getItem("isBindPhone")
            ? localStorage.getItem("isBindPhone")
            : 0;
          let flag = Date.now() - readTime > 1000 * 60 * 60 * 12;
          if (flag) {
            this.bindPhone();
          }
        }
      });
    }
    // localStorage.setItem('MINT_auth','3CwB8U7GyFBN5rsn0e2PRfeocpK41MQ/byfXUF6vxMAcWcjIo6CPcx+USC7/pjvg58VFMtI/Ex9JQZqMqluxvW8eVZj8/J9OJYBpHcvnRg3g5tsgXwg8VJf8tO/dR8Mf4A18WPZuYo3tslkZmmmO3Ly5sYXFElzAWhLdReKnhob51FdNhs3P4T3+7rUYtdHmSyegEFhiX7oOm/kG1hGsJuLDlw3a+YVhpiZG3JPHACagxQ5FZkfJrhljh7XGOO7PogoYr3/m2nYNyE8sHlNPOhxDh7RfomYK4rbFzuzFoUBJI8Ewc3zq53E3X04GohhC4XeF21oC92YzngdewS2cpFdXZgIfitY5T7SHGaBziuQ01uWTV/EhNVg3WIKMrQV0CgYTIxHjXwPmfCRHA84T/ZRq9c2OCRlaL6gfl9Ig41DFvfK2rMvdw4ppBGleocYBQ+1j+grWrnvG9z2ysaRRuO4RkoA06O9ExtnIBOEEiLtL0pR2L/YArMztnt7EYkUY+4NMYHDVjuc=')
    // S6roXKXo2NQnURd078egQHYqYoPtJRp+kQ+0Ad6OtT9ClMiGxBgSs6GBXaco3otTvlc835428qRq2WbJxYQjWdihEqH8xSbebWPJWWtilcSjWXlG9NmJSxywjvz87qgrwHoqHvXtnafnDCphL6WLQQ==
    this.getRotateImageList();
    this.getPageInformation();
    this.getSymbolsPageList();
    this.SymbolsList = localStorage.getItem("Symbols")
      ? JSON.parse(atob(localStorage.getItem("Symbols")))
      : [];
    this.Time = setInterval((_) => {
      this.getSymbolsPageList();
    }, 6000);
    // this.showDownLoadPop()
    this.vCoinsList = this.SymbolsList.slice(0, 12);
    console.log(this.vCoinsList, "vCoins");
    // second-panel
    this.secondList = [
      {
        title: "Reserve Description",
        desc: "We promise that the platform holds alluser assets at a reserve ratio of at least",
        img: require("../assets/pc/组 11757.png"),
      },
      {
        title: "safe cold storage",
        desc: "We securely store most of our digitalassets in offline multi-signature wallets",
        img: require("../assets/pc/组 11758.png"),
      },
      {
        title: "User Protection Fund",
        desc: "We have a user protection fund worth 300 million USDT, providing an extra layer of protection for potential security risks.",
        img: require("../assets/pc/组 11759.png"),
      },
    ];
  },
  methods: {
    confirmNotice() {
      let data = {
        UserId: this.MINT_auth.UserId,
        Action: "NoticeRead",
        ActionValue: this.NoticeId,
      };
      UpdateUserEntity(data).then((r) => {
        if (r.data.Code == 200) {
          localStorage.setItem("isRead", Date.now());
          this.showNotice = false;
        }
      });
    },
    // // 1===》 获取最新的通知公告(首页弹窗)
    async getLatestNotice() {
      let data = { UserId: this.MINT_auth.UserId };
      let readTime = localStorage.getItem("isRead")
        ? localStorage.getItem("isRead")
        : 0;
      console.log(Date.now() - readTime);
      let flag = Date.now() - readTime > 1000 * 60 * 30;
      console.log(flag);
      if (flag) {
        await GetLatestNotice(data).then((r) => {
          console.log(">>>>>>GetLatestNotice");
          console.log(r.data);
          if (r.data.Code == 200) {
            this.showNotice = true;
            this.NoticeId = r.data.Data.NewsId;
            this.NoticeIframURL = r.data.Data.H5Url;

            console.log(r.data.Data);
            var _clientHeight = document.documentElement.clientHeight;
            setTimeout((_) => {
              const deviceWidth = document.documentElement.clientWidth;
              const deviceHeight = document.documentElement.clientHeight;
              this.$refs.NoticeIfram.style.height = "600px";
            }, 10);
          } else {
            localStorage.setItem("isRead", Date.now());
          }
        });
      }
    },
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
    closrSearch() {
      this.nShow = !this.nShow;
    },
    toKline(i) {
      localStorage.setItem("SymbolsIdTiem", JSON.stringify(i));
      localStorage.setItem("path", "deal");
      this.$router.push({
        path: "/trad",
        query: {
          SymbolsId: i.SymbolsId,
        },
      });
    },
    // 实现移动端拖拽
    down(event) {
      this.flags = true;
      var touch;
      if (event.touches) {
        touch = event.touches[0];
      } else {
        touch = event;
      }
      console.log("鼠标点所在位置", touch.clientX, touch.clientY);
      console.log(
        "div左上角位置",
        event.target.offsetTop,
        event.target.offsetLeft
      );
      dx = touch.clientX - event.target.offsetLeft;
      dy = touch.clientY - event.target.offsetTop;
    },
    move() {
      console.log("屏幕大小", screenWidth, screenHeight);
      if (this.flags) {
        var touch;
        if (event.touches) {
          touch = event.touches[0];
        } else {
          touch = event;
        }
        // 定位滑块的位置
        this.position.x = touch.clientX - dx;
        this.position.y = touch.clientY - dy + "px";
        // 限制滑块超出页面
        console.log("屏幕大小", screenWidth, screenHeight);
        // if (this.position.x < 0) {
        //   this.position.x = 0
        // } else if (this.position.x > screenWidth - touch.target.clientWidth) {
        //   this.position.x = screenWidth - touch.target.clientWidth
        // }
        // if (this.position.y < 0) {
        //   this.position.y = 0
        // } else if (this.position.y > screenHeight - touch.target.clientHeight) {
        //   this.position.y = screenHeight - touch.target.clientHeight
        // }
        //阻止页面的滑动默认事件
        let service = document.getElementById("service");
        service.addEventListener(
          "touchmove",
          function (event) {
            event.preventDefault();
          },
          false
        );
      }
    },
    //鼠标释放时候的函数
    end() {
      console.log("end");
      this.flags = false;
    },
    // https://124.221.191.238:9080/News/CustomerService/{语言版本标识zh-CN/zh-hk/en/jpn}
    CustomerService() {
      let url =
        this.$config.imgPath +
        `/News/CustomerService/${this.lang}?token=${this.MINT_auth.UserId}`;
      location.href = url;
    },

    getRotateImageList() {
      GetRotateImageList({ ShowWay: 0 }).then((r) => {
        console.log(r);
        const { data } = r;
        if (data.Code == 200) {
          this.banner = data.Data;
          let autoplay;
          if (data.Data.length > 1) {
            autoplay = {
              delay: 2500,
              disableOnInteraction: false,
            };
          } else {
            autoplay = false;
          }
          setTimeout((_) => {
            var mySwiper = new Swiper(".swiper-container", {
              direction: "horizontal",
              slidesPerView: "auto",
              centeredSlides: true,
              watchSlidesProgress: true,
              paginationClickable: true,
              // slidesPerView: 1.1,
              spaceBetween: 10,
              loop: true,
              autoplay,
              pagination: {
                el: ".swiper-pagination",
              },
            });
          }, 500);
        }
      });
    },
    getPageInformation() {
      let data = {
        PageSize: 1,
        PageIndex: 1,
        UserId: this.MINT_auth ? this.MINT_auth.UserId : "default",
      };
      GetInformationPageList(data).then((r) => {
        if (r.data.Code == 200) {
          //  console.log(r.data)
          this.information = r.data.Data.rows[0];
        }
      });
    },
    //region 行情 => 获取全部symbol的交易行情列表
    getSymbolsPageList() {
      let data = {
        PageIndex: 1,
        PageSize: 100,
        AreaId: "Default",
        Sidx: "SortCode",
        Sord: "desc",
        KeyWord: "Default",
        Flag: "2",
      };
      GetSymbolsPageList(data).then((r) => {
        console.log(r.data);
        if (r.data.Code == 200) {
          this.SymbolsList = r.data.Data.DataList;
          let hasSymbolsIdTiem = localStorage.getItem("SymbolsIdTiem");
          if (!hasSymbolsIdTiem) {
            localStorage.setItem(
              "SymbolsIdTiem",
              JSON.stringify(this.SymbolsList[0])
            );
          }
          let Symbols = btoa(JSON.stringify(this.SymbolsList));
          localStorage.setItem("Symbols", Symbols);
          let arr = [];
          r.data.Data.DataList.map((_) => {
            arr.push({
              SymbolsId: _.SymbolsId,
              BaseCurrency: _.BaseCurrency,
              QuoteCurrency: _.QuoteCurrency,
            });
          });
          // this.SymbolsList.push(...r.data.Data.DataList) ;
          let search = btoa(JSON.stringify(arr));
          localStorage.setItem("search", search);
        }
      });
    },
    gainRate(i) {
      let str = i.RisingValue > 0 ? `+` : ``;
      let num = (((i.ClosePrice - i.OpenPrice) / i.OpenPrice) * 100).toFixed(2);
      return str + num;
    },
    bindPhone() {
      Dialog.confirm({
        title: this.$t("base.bindPhone"),
        message: this.$t("base.bindPhoneTips"),
      })
        .then(() => {
          this.$router.push({
            path: "/bind_account",
            query: {
              type: "mobile",
              redirect: "/",
            },
          });
        })
        .catch(() => {
          localStorage.setItem("isBindPhone", Date.now());
          // on cancel
        });
    },
    // toDetail(i) {
    //   if (this.MINT_auth && i.isread != 1) {
    //     let data = {
    //       UserId: this.MINT_auth.UserId,
    //       Action: "NewsRead",
    //       ActionValue: i.newsid,
    //     };
    //     UpdateUserEntity(data).then((r) => {
    //       if (r.data.Code == 200) {
    //         this.$router.push({
    //           path: "/H5",
    //           query: {
    //             url: this.$config.imgPath + `/News/Detail/${i.newsid}`,
    //             title: this.$t("news.detailTitle"),
    //           },
    //         });
    //       }
    //     });
    //   } else {
    //     this.$router.push({
    //       path: "/H5",
    //       query: {
    //         url: this.$config.imgPath + `/News/Detail/${i.newsid}`,
    //         title: this.$t("news.detailTitle"),
    //       },
    //     });
    //   }
    // },
    newsMore() {
      this.$router.push({
        path: "/property",
        query: {
          index: 7,
        },
      });
      this.$store.commit("setActiveIndex", 7);
    },
    toTrad(i) {
      localStorage.setItem("path", "deal");
      localStorage.setItem("SymbolsIdTiem", JSON.stringify(i));
      this.$router.push({
        path: "/trad",
        query: {
          SymbolsId: i.SymbolsId,
        },
      });
    },
    // showDownLoadPop(){
    //   let downloadTime = localStorage.getItem('download');
    //   if(!downloadTime){
    //     this.showDownLoad = true;
    //   }else{
    //     console.log(Date.now() - Number(downloadTime)>(1000*60*60*12))
    //    this.showDownLoad =  Date.now() - Number(downloadTime)>(1000*60*60*12)
    //   }
    // },
    // closeDownLoad(e){
    //   this.showDownLoad = false;
    // },
    downloadApp(type) {
      let downloadUrl = this.$config.imgPath + `/WebUser/Download/${this.lang}`;
      const { appDownloadUrl } = config;
      if (type == 1) {
        // android 下载地址
        downloadUrl = appDownloadUrl.android || "https://www.google.com";
      } else {
        // ios 下载地址
        downloadUrl = appDownloadUrl.ios || "https://www.apples.com";
      }
      window.open(downloadUrl);
    },
    toNavDetail(i) {
      if (!this.MINT_auth) {
        this.$router.push("/login");
        return;
      }
      if (i.url) {
        this.$router.push({
          path: i.url,
        });
      } else {
        this.$toast(this.$t("base.coming"));
      }
    },
    toQuick() {
      if (!this.QuickCoinURL) {
        this.$router.push("/login");
      }
    },
    toBuyCoins() {
      this.$router.push("/coins");
    },
    toQuickCoin() {
      console.log(1232);
      if (this.MINT_auth) {
        console.log(11);
        GetQuicklyBuyCoinsUrl({ UserId: this.MINT_auth.UserId }).then((r) => {
          console.log(r);
          if (r.data.Code == 200) {
            this.QuickCoinURL = r.data.Data;
            // console.log('>>>>132213>>>>')
            // window.open(r.data.Data)
            // this.$router.push({
            //   path:"/H5",
            //   query:{
            //     url:r.data.Data,
            //     title:this.$t('base.Quickly')
            //     }
            // })
          }
        });
      } else {
      }
    },
    goPage(i) {
      const listBox = document.querySelector(".list-box");
      // this.transformWidth = 0
      if (i === 1) {
        // 向左
        if (this.transformWidth == 0) {
          return;
        }
        this.transformWidth += 1200;
      } else {
        // 向右
        console.error(this.transformWidth);
        if (this.transformWidth == -2400) {
          return;
        }
        this.transformWidth -= 1200;
      }
      listBox.style.transform = `translateX(${this.transformWidth}px)`;
    },
  },
  destroyed() {
    console.log("清除定时器");
    clearInterval(this.Time);
    // // document.body.style.backgroundColor='#fff'
  },
};
</script>
<style lang="less" scoped>
.index_wrap {
  .header {
    color: #f1f1f1;
    height: 46px;
    line-height: 46px;
    // position: fixed;
    display: flex;
    align-items: center;
    // top: 0;
    width: 100%;
    .bot_br;
    background-color: transparent;
    // z-index: 1000;
    // &.show_download{
    //   top:100px;
    // }
    > img {
      height: 46px;
      margin: 0 auto;
      display: block;
    }
    .header_right {
      position: absolute;
      right: 20px;
      // width: 50%;
      display: flex;
      align-items: center;
      background: transparent;
      // justify-content: flex-end;
      img {
        height: 46px;
        margin-left: 40px;
      }
    }
    .logo {
      position: absolute;
      left: 20px;
      color: rgb(7, 120, 215);
      font-size: 0.4rem;
      font-weight: 900;
    }
    .right-title {
      // position: absolute;
      // right: 20px;

      color: rgb(7, 120, 215);
      font-size: 0.4rem;
      font-weight: 900;
    }
  }
  .content {
    // padding-top: 90px;
    background-color: @background-color;
    .sam1 {
      margin-left: 15px;
      margin-top: -10px;
      border-top-left-radius: 30px;
      font-size: 26px;
      border-bottom-right-radius: 30px;
      padding: 10px;
      background-color: #c72aff;
    }
    .first_wrap {
      background: url(../assets/pc/蒙版组 11@3x.png) no-repeat;
      background-size: 100% 100%;
      border-radius: 15px;
      height: 925px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      @media (max-width: 1920px) and (max-height: 1080px) {
        /* 样式代码 */
        height: 680px;
      }
      .left {
        .title1 {
          width: 960px;
          height: 144px;
          font-family: Source Han Sans CN;
          font-size: 64px;
          color: #fff;
          text-align: left;
          line-height: 80px;
          opacity: 1;
        }
        .title2 {
          width: 1034px;
          height: 71px;
          font-family: Source Han Sans CN;
          font-size: 54px;
          color: #fff;
          text-align: left;
          opacity: 1;
        }
        .try {
          display: flex;
          justify-content: flex-start;
          align-items: center;
          color: @font_3_color;
          text-align: center;
          margin-top: 100px;
          input {
            width: 400px;
            height: 67px;
            background: rgba(0, 0, 0, 0.26);
            border: 1px solid #373737;
            border-radius: 20px;
            opacity: 1;
          }
          .btn {
            width: 177px;
            height: 67px;
            line-height: 67px;
            background: #191919;
            border: 1px solid #373737;
            border-radius: 34px;
            opacity: 1;
            margin-left: 20px;
          }
        }
        .text-title {
          width: 360px;
          height: 24px;
          font-family: Source Han Sans CN;
          font-size: 24;
          color: #606060;
          text-align: left;
          opacity: 1;
          margin: 20px 0;
        }
        .download-app {
          display: flex;
          justify-content: flex-start;
          align-items: center;
          .google,
          .apple {
            width: 400px;
            height: 67px;
            line-height: 67px;
            background: rgba(0, 0, 0, 0.26);
            border: 1px solid #373737;
            border-radius: 10px;
            opacity: 1;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            color: @font_3_color;
            cursor: pointer;
            img {
              width: 40px;
              height: 40px;
              margin: 0 20px;
            }
          }
          .apple {
            margin-left: 20px;
          }
        }
      }
      .right {
        width: 300px;
        height: 600px;
        opacity: 1;
        img {
          width: 100%;
          height: 100%;
        }
      }
    }
    .swiper_wrap {
      background-color: transparent;
      border-radius: 15px;
      height: 630px;
      // padding: 10px 20px;
      .swiper-container {
        .swiper-slide {
          width: 100%;
          // padding: 0px 20px;
          height: 600px;
          // border-radius: 20px;
          overflow: hidden;
          // height: 100%;
        }
        img {
          height: 100%;
          width: 100%;
        }
      }
    }
    .description1 {
      margin: 50px 0;
      // width: 1364px;
      height: 60px;
      line-height: 60px;
      font-family: Source Han Sans CN;
      font-size: 50px;
      color: #fff;
      text-align: center;
      opacity: 1;
    }
    .notice {
      // background-color: @panel_color;
      width: 100%;
      height: 113px;
      background: #000;
      border: 1px solid #5a5a5a;
      border-radius: 0px;
      opacity: 1;

      border-radius: 20px;
      padding: 20px 20px 20px 0;
      // margin: 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      .notice_left {
        width: 189px;
        height: 72px;
        line-height: 72px;
        background: #000;
        border: 1px solid #b7b7b7;
        border-radius: 36px;
        opacity: 1;
        font-size: 28px;
        text-align: center;
        color: @font_3_color;
      }
      .notice_right {
        width: 90%;
        padding-left: 20px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        .notice_ico {
          width: 64px;
          img {
            width: 100%;
          }
        }
        .title {
          display: flex;
          // justify-content: space-between;
          span {
            font-size: 30px;
            &:nth-of-type(1) {
              color: @main_color;
              // width: 85%;
              display: block;
              overflow: hidden;
              text-overflow: ellipsis;
              display: -webkit-box;
              -webkit-line-clamp: 1;
              -webkit-box-orient: vertical;
            }
          }
          img {
            margin-left: 10px;
            height: 22px;
          }
        }
        > div:nth-of-type(2) {
          width: 85%;
          margin-left: 30px;
          // position: relative;
          .doc {
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 1;
            -webkit-box-orient: vertical;
            color: @font_color;
            font-family: Source Han Sans CN;
            font-size: 32px;
            color: #fff;
            text-align: left;
            opacity: 1;
          }
        }
      }
    }
    .nav-box {
      border: 4px solid transparent;
      border-radius: 16px;
      position: relative;
      background-color: #222;
      background-clip: padding-box; /*important*/
      .nav {
        // background-color: @panel_color;
        border: 3px solid;
        border-image: linear-gradient(
            to right,
            #00eeff,
            #8e58f9,
            #4a94e8,
            #1be4df
          )
          1;
        border-left: none;
        border-right: none;
        // border-radius: 60px;
        // padding: 20px;
        // margin: 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        width: 100%;

        .center {
          height: 200px;
          // width: calc(100% - 400px);
          > ul {
            display: flex;
            justify-content: flex-start;
            flex-wrap: wrap;
            width: 1200px;
            li {
              // flex: 1;
              width: 25%;
              margin: 25px 0px;
              text-align: center;
              position: relative;
              .pic {
                width: 100px;
                height: 100px;
                text-align: center;
                margin: 0 auto 10px;
                position: relative;
                &.hot {
                  &::before {
                    content: "";
                    display: block;
                    width: 52px;
                    height: 30px;
                    background: url(../assets/images/common/hot.png) center
                      no-repeat;
                    background-size: contain;
                    position: absolute;
                    top: -10px;
                    right: -30px;
                  }
                }
                img {
                  width: 100%;
                }
              }
              p {
                font-size: 26px;
                color: @font_color;
              }
            }
          }
        }
        .left,
        .right {
          width: 400px;
          height: 100%;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .left {
          margin-left: -200px;
        }
        .right {
          transform: rotate(180deg);
          margin-right: -200px;
        }
      }
    }
    .nav-box::before {
      content: "";
      position: absolute;
      top: 0;
      right: 0;
      left: 0;
      bottom: 0;
      z-index: -1;
      margin: -4px;
      border-radius: inherit; /*important*/
      background: linear-gradient(to right, #8f41e9, #578aef);
    }

    .list-wrapper {
      display: flex;
      justify-content: space-around;
      align-items: center;
      .list-box-wrapper {
        width: 1200px;
        overflow-x: hidden;
        .list-box {
          margin-top: 60px;
          display: flex;
          justify-content: flex-start;
          align-items: center;
          width: 3600px;
          transition: all 0.5s;
          // overflow: scroll;
          .list-item {
            width: 300px;
            height: 383px;
            background: #232425;
            border: 1px solid #404040;
            border-radius: 10px;
            opacity: 0.8;
            text-align: center;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-right: 20px;
            &:last-of-type {
              margin-right: 0;
            }
            .content {
              width: 100%;
              // height: 100%;
              background: transparent;

              .icon {
                width: 50px;
                height: 50px;
                margin: 0 auto;
                img {
                  width: 100%;
                  height: 100%;
                }
              }
              .name {
                font-size: 24px;
                color: #ffffff;
                text-align: center;
                margin-top: 20px;
              }
              .num {
                font-size: 24px;
                color: @main_color;
                text-align: center;
                margin-top: 10px;
              }
              .percent {
                margin: 0 auto;
                width: 80%;
                height: 40px;
                line-height: 40px;
                font-size: 24px;
                color: #000;
                background: #07eefe;
                text-align: center;
                margin-top: 10px;
                border-radius: 10px;
              }
            }
          }
        }
      }
      .left-arrow,
      .right-arrow {
        width: 100px;
        img {
          width: 100%;
          height: 100%;
        }
      }
    }
    .quick_coin {
      background-color: @panel_color;
      border-radius: 20px;
      padding: 20px;
      padding-right: 0;
      margin: 20px;
      display: flex;
      justify-content: space-between;
      > div {
        display: flex;
        align-items: center;
        &:nth-of-type(1) {
          display: flex;
          align-items: center;
          .pic {
            width: 76px;
            img {
              width: 100%;
            }
            & + div {
              margin-left: 20px;
              p {
                &:nth-of-type(1) {
                  color: @font_color;
                  font-size: 32px;
                  font-weight: 500;
                }
                &:nth-of-type(2) {
                  color: @font_color;
                  font-size: 24px;
                }
              }
            }
          }
        }
        &:nth-of-type(2) {
          margin-right: 30px;
          img {
            height: 52px;
          }
        }
      }
    }
    .description2 {
      // width: 998px;
      margin: 50px 0;
      height: 72px;
      font-family: Source Han Sans CN;
      font-size: 72px;
      color: #fff;
      text-align: center;
      opacity: 1;
    }
    .description3 {
      // width: 1220px;
      margin: 50px 0 100px 0;
      height: 41px;
      font-family: Source Han Sans CN;
      font-size: 36px;
      color: #fff;
      text-align: center;
      opacity: 1;
    }
    .con_list {
      margin: 20px auto;
      padding: 20px 0px;
      // padding-bottom: 120px;
      // background-color: @panel_color;
      border-top-right-radius: 24px;
      border-top-left-radius: 24px;
      width: 90%;
      // border-radius-top: 20px;
      .con_title {
        height: 122px;
        color: @font_color;
        .bot_br;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .lf {
          font-size: 32px;
          img {
            width: 30px;
          }
        }
        .fr {
          color: @font_color;
          font-size: 28px;
          margin-right: 40px;
          img {
            width: 32px;
          }
        }
        border-bottom: 1px solid #252525;
      }
      ul {
        display: flex;
        justify-content: space-between;
        margin-bottom: 2px;
        padding: 10px 0;
        align-items: center;
        border-bottom: 1px solid #252525;
        // .bot_br;
        li {
          width: 33.33%;
          // padding-top: 20px;
          &:nth-of-type(1) {
            div {
              p {
                &:nth-of-type(1) {
                  font-weight: 500;
                  font-size: 32px;
                  color: @font_3_color;
                  span {
                    font-size: 18px;
                    color: @minor-font-color;
                    margin-left: 5px;
                  }
                }
                &:nth-of-type(2) {
                  font-weight: 400;
                  // color: @minor-font-color;
                  color: @main_color;
                  // color: #1DD368;
                }
              }
            }
          }
          &:nth-of-type(2) {
            p {
              font-weight: 400;
              color: @main_color;
              font-size: 35px;
            }
            text-align: center;
          }
          &:nth-of-type(3) {
            p {
              &:nth-of-type(1) {
                font-weight: 600;
                font-size: 36px;
                color: @font_color;
              }
              &:nth-of-type(2) {
                font-weight: 600;
                color: @minor-font-color;
                font-size: 20px;
              }
            }
            text-align: center;
          }
          &:nth-of-type(4) {
            div {
              width: 180px;
              height: 60px;
              display: flex;
              align-items: center;
              justify-content: center;
              color: @font_3_color;
              border-radius: 8px;
              &.up {
                color: @up-color;
                
              }
              &.down {
                color: @down-color;
              }
              img{
                  margin-right: 20px;
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
  .second-wrapper {
    height: 300px;
    margin-top: 20px;
    margin-bottom: 20px;
    // height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    .content {
      width: 87%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      .item {
        width: 400px;
        height: 320px;
        background: url(../assets/pc/组 11757@3x.png) no-repeat;
        background-size: 100% 100%;
        .title {
          margin-top: 150px;
          max-width: 354px;
          height: 36px;
          font-family: Source Han Sans CN;
          font-size: 36px;
          color: #fff;
          text-align: center;
          opacity: 1;
        }
        .desc {
          margin-top: 20px;
          max-width: 448px;
          height: 107px;
          font-family: Source Han Sans CN;
          font-size: 24px;
          color: #fff;
          text-align: center;
          line-height: 24px;
          opacity: 1;
        }
      }
    }
  }
  .service {
    position: fixed;
    bottom: 200px;
    border-radius: 50%;
    right: 20px;
    padding: 10px;
    width: 80px;
    height: 80px;
    z-index: 1000;
    // img{
    //   width: 100%;
    // }
    overflow: hidden;
    background-color: @main_color;
  }
  .notice_wrap {
    padding: 20px 0;
    .notice_ifram {
      padding: 20px;
      height: 700px;
      overflow-y: scroll;
      iframe {
        max-height: 660px;
      }
      max-height: 700px;
    }
    .btns {
      padding: 10px 30px 20px;
      text-align: center;
      button {
        height: 88px;
        width: 80%;
        border-radius: 10px;
        background-color: @main_color;
        color: @font_color;
      }
    }
  }
}
</style>
