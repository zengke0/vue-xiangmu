<template>
  <article class="index">
      <v-swipe :list="lunbos"></v-swipe>
      <v-nav></v-nav>
  </article>
</template>

<script>
import Cswipe from '../common/swipe.vue';
import Cnav from './nav.vue';
export default {
  data() {
    return {
      lunbos: []
    };
  },
  methods: {
    // 获取轮播图数据的方法
    getLunbos() {
        let url = 'http://139.199.192.48:8888/api/getlunbo';
        this.$http.get(url).then(rep => {
          let body = rep.body;
          if(body.status == 0) {
            // 因为公共的轮播图使用的图片地址属性名为src，这里的图片地址名为img，所以转换一下
            this.lunbos = body.message.map(item => (item.src = item.img) && item);
            // 上面的写法和这里一个意思
            // this.lunbos = body.message.map(item => {
            //   item.src = item.img;
            //   return item;
            // });
          }
        });
    }
  },
  // 当组件初始化完毕，数据和事件都也绑定好了的时候被自动调用
  created() {
    this.getLunbos();
  },
  components: {
    'v-swipe': Cswipe,
    'v-nav': Cnav
  }
}
</script>

<style>

</style>

