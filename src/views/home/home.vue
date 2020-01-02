<template>
  <div id="home">
    <navbar class="home-nav">
      <div slot="center">购物街</div>
    </navbar>
    <Scroll
      class="content"
      ref="scroll"
      :probeType="3"
      @scroll="contentscroll"
      :pullUpLoad="true"
      @pullingUp="contentpullingUp"
    >
      <homeSwiper :banners="banners"></homeSwiper>
      <recommendViews :recommends="recommends"></recommendViews>
      <feature></feature>
      <tabcontrol :titles="['流行','新款','精选']" class="tabcontrol" @tabClick="tabClick"></tabcontrol>
      <goodslist :goods="showGoods"></goodslist>
    </Scroll>
    <Backtop @click.native="backclick" v-show="isshowbacktop"></Backtop>
  </div>
</template>
<script>
import navbar from "components/common/navbar/navbar";
import homeSwiper from "./childComps/homeSwiper";
import recommendViews from "./childComps/recommendView";
import feature from "./childComps/feature";
import tabcontrol from "components/content/tabControl/tabControl";
import goodslist from "components/content/goods/goodslist";
import Scroll from "components/common/scroll/Scroll";
import Backtop from "components/content/backTop/BackTop";
import { getHomeMultidata, getgoodslist } from "network/home";

export default {
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      currentType: "pop",
      isshowbacktop: false
    };
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    }
  },
  components: {
    navbar,
    homeSwiper,
    recommendViews,
    feature,
    tabcontrol,
    goodslist,
    Scroll,
    Backtop
  },
  created() {
    this.getHomeMultidata();
    this.getgoodslist("pop");
    this.getgoodslist("new");
    this.getgoodslist("sell");
  },
  methods: {
    /**
     * 时间监听的方法
     */

    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
    },
    backclick() {
      this.$refs.scroll.scrollTo(0, 0, 1000);
    },
    contentscroll(position) {
      this.isshowbacktop = -position.y > 1000;
    },
    contentpullingUp(){
      this.getgoodslist(this.currentType)
      this.$refs.scroll.scroll.refresh()
    },
    /**
     * 网络请求的方法
     */
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getgoodslist(type) {
      const page = this.goods[type].page + 1;
      getgoodslist(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        this.$refs.scroll.finishPullUp()
      });
    },
  }
};
</script>
<style scoped>
#home {
  padding-top: 44px;
  height: 100vh;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}
.tabcontrol {
  top: 44px;
  z-index: 9;
}
.content {
  position: absolute;
  overflow: hidden;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}
</style>