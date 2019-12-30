<template>
  <div id="home">
    <navbar class="home-nav">
      <div slot="center">购物街</div>
    </navbar>
    <homeSwiper :banners="banners"></homeSwiper>
    <recommendViews :recommends="recommends"></recommendViews>
    <feature></feature>
    <tabcontrol :titles="['流行','新款','精选']" class="tabcontrol" @tabClick="tabClick"></tabcontrol>
    <goodslist :goods='showGoods'></goodslist>
  </div>
</template>
<script>
import navbar from "components/common/navbar/navbar";
import homeSwiper from "./childComps/homeSwiper";
import recommendViews from "./childComps/recommendView";
import feature from "./childComps/feature";
import tabcontrol from "components/content/tabControl/tabControl";
import goodslist from "components/content/goods/goodslist"
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
      currentType:'pop'
    };
  },
  computed: {
    showGoods(){
      return this.goods[this.currentType].list
    }
  },
  components: {
    navbar,
    homeSwiper,
    recommendViews,
    feature,
    tabcontrol,
    goodslist,
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
    tabClick(index){
      switch(index){
        case 0:
          this.currentType='pop'
          break
        case 1:  
          this.currentType='new' 
          break
        case 2:
          this.currentType='sell'  
          break
      }
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
      });
    },
  }
};
</script>
<style>
#home {
  padding-top: 44px;
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
  position: sticky;
  top: 44px;
  z-index:9;
}
</style>