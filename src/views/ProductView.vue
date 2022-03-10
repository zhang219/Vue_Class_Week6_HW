<template>
  <h2>產品列表</h2>
  <div class="container"><!--定義容器-->
    <!--row 決定內層的數量-->
    {{ product }}
    <img :src="product.imageUrl" alt="">
  </div><!--定義容器-->
</template>

<script>
export default {
  data() {
    return {
      product: [],
    };
  },
  methods: {
    getProduct() {
      // $router -> 方法
      // $route -> 物件 -> 取值
      console.log(this.$route);
      const { id } = this.$route.params; // 亂數參數 id 傳遞
      this.$http(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/product/${id}`).then((res) => {
        this.product = res.data.product;
      });
    },
  },
  mounted() {
    this.getProduct();
  },
};
</script>
