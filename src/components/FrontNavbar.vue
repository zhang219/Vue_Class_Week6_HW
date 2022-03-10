<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
        <router-link class="navbar-brand" to="/">ZhangZhang</router-link> <!--盡量不要出現a連結-->
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link class="nav-link" to="/">首頁</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/products">產品列表</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/cart">購物車</router-link>
            </li>
          </ul>
        </div>
        <button type="button" class="btn btn-primary">
          結帳
          <span class="badge rounded-pill bg-danger">{{ cartData.carts.length }}</span>
        </button>
    </div>
  </nav>
</template>

<script>
import emitter from '@/libs/emitter';

export default {
  data() {
    return {
      cartData: {
        carts: [], // 預先定義一個陣列，讀取 length 數字不會出錯
      }, // 購物車列表
    };
  },
  methods: {
    getCart() {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          console.log('cart:', res);
          this.cartData = res.data.data; // 購物車內容data-> data -> carts(已加入購物車的內容)->total
        });
    },
  },
  mounted() {
    this.getCart(); // 初始化過程中就須取得
    emitter.on('get-cart', () => {
      this.getCart();
    }); // 當被觸發時，監聽重新取得購物車品項
  },
};
</script>
