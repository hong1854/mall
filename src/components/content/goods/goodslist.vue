<template>
  <div class="goodslist">
    <goodslistitem v-for="item in goods" :goodsitem="item"/>
    
  </div>
</template>
<script>
import goodslistitem from './goodslistitem'
export default {
  props:{
    goods:{
      type:Array,
      default(){
        return []
      }
    }
  },
  components:{
    goodslistitem,
  }
}
</script>
<style>
  .goodslist {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    padding: 2px;
  }
  
</style>