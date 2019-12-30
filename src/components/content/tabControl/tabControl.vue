<template>
  <div class="tabcontrol">
    <slot>
      <div
        v-for="(item,index) in titles"
        :key="item.titles"
        class="tabcontrolitem"
        :class="{active:index ===currentindex}"
        @click="itemclick(index)"
      >
        <span>{{item}}</span>
      </div>
    </slot>
  </div>
</template>
<script>
export default {
  data() {
    return {
      currentindex: 0
    };
  },
  props: {
    titles: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  methods:{
    itemclick(index){
      this.currentindex=index;
      this.$emit('tabClick',index)
    }
  }
};
</script>
<style>
.tabcontrol {
  display: flex;
  text-align: center;
  font-size: 15px;
  height: 40px;
  line-height: 40px;
  background-color: #fff;
}
.tabcontrolitem {
  flex: 1;
}
.tabcontrolitem span {
  padding: 8px;
}
.active {
  color: var(--color-high-text);
}
.active span {
  border-bottom: 1.5px solid var(--color-tint);
}
</style>