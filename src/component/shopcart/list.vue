<template>
  <article class="shopcart-list">

    <v-title :title="title"></v-title>

    <!-- 商品列表 -->
    <div class="goods" v-for="item in shopcartList" :key="item.id">
      <mt-switch class="switch" v-model="item.selected"></mt-switch> 
      <img class="img" :src="item.src">
      <div class="inforight">
        <h4 class="mui-ellipsis-2">{{ item.title }}</h4>
        <div class="bottom">
          <ul>
            <li>￥{{ item.sell_price }}</li>
            <li>
              <div class="mui-numbox"> <button class="mui-btn mui-btn-numbox-minus">-</button> <input class="mui-input-numbox" type="number"> <button class="mui-btn mui-btn-numbox-plus">+</button> </div>
            </li>
            <li>
              <a href="javascript:void(0)">删除</a>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- 总价 -->
    <div class="total">
      <div class="total_val">
        <ul>
          <li>总计（不含运费）</li>
          <li>已勾选商品10件,总价:￥1000元</li>
        </ul>
      </div>
      <div class="total_btn">
        <mt-button type="primary">付 款</mt-button>
      </div>
    </div>

  </article>
</template>

<script>
  import config from '../../js/config.js';
  import Ctitle from '../common/title.vue';
  import goodsStorage from '../../js/model/goods.js';

  export default {

   data() {
     return {
      title: '购物车',
      shopcartList: []
     };
   },

   methods: {
     // 获取购物车商品数据
     getShopcartList() {
      let url = config.shopcartList + goodsStorage.getIDList();
      this.$http.get(url).then(rep => {
        let body = rep.body;
        // 因为请求回来的图片没有域名，所以加一下
        // 每个商品都额外添加一个selected属性用来控制该商品的选取，默认选取状态
        if(body.status == 0) {
          this.shopcartList = body.message.map(item => {
            item.selected = true;
            item.src = config.imgDomain + item.thumb_path;
            return item;
          });
        }
      });
     }
   },

   created() {
     this.getShopcartList();
   },

   components: {
     'v-title': Ctitle
   }

 };
</script>

<style lang="less">
  .shopcart-list {
    .goods {
      border-bottom: 1px solid rgba(0, 0, 0, 0.3);
      height: 102px;
      display: flex;
      padding: 5px;
      .switch {
        flex: 0 0 52px;
      }
      .img {
        margin-left: 5px;
        height: 75px;
        width: 75px;
        flex: 0 0 85px;
      }
      .inforight {
        margin-left: 5px;
        flex: 1;
      }
      .inforight ul {
        padding-left: 0px;
      }
      .inforight li {
        list-style: none;
        display: inline-block;
      }
      .inforight h4 {
        color: #0094ff;
        font-size: 14px;
      }
      .bottom li:first-child {
        color: red;
        margin-right: 5px;
      }
      .bottom li:last-child {
        margin-left: 5px;
      }
    }
    .total {
      height: 84px;
      background-color: rgba(0, 0, 0, 0.1);
      display: flex;
      padding: 5px 14px;
      ul {
		    padding-left: 0px;
		    margin: 14px 0;
		    li {
		      list-style: none;
		      font-size: 14px;
		    }
		  }
		  &_val {
		  	flex: 1;
		  }
		  &_btn {
		  	flex: 0 0 60px;
	      margin: 18px 0 0 0;
		  }
    }
  }
</style>