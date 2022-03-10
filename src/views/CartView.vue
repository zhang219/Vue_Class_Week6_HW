<template>
  <div class="container">
    <h2>購物車</h2>
    <table class="table align-middle">
      <thead>
        <tr>
          <th>圖片</th>
          <th>商品名稱</th>
          <th>價格</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td style="width: 200px">
            <div
              :style="{
                backgroundImage: `url(${product.imageUrl})`,
              }"
              style="
                height: 100px;
                background-size: cover;
                background-position: center;
              "
            ></div>
            <!-- 插入背景圖片(物件形式) -->
          </td>
          <td>{{ product.title }}</td>
          <td>
            <div class="h5" v-if="product.price === product.origin_price">
              {{ product.price }} 元
            </div>
            <div v-else>
              <del class="h6">原價 {{ product.origin_price }} 元</del>
              <div class="h5">現在只要 {{ product.price }} 元</div>
            </div>
          </td>
          <td>
            <div class="btn-group btn-group-sm">
              <button
                type="button"
                class="btn btn-outline-secondary"
                :disabled="isLoadingItem === product.id"
                @click="openProductModal(product.id)"
              >
                查看更多
              </button>
              <button
                type="button"
                class="btn btn-danger"
                @click="addToCart(product.id)"
                :disabled="isLoadingItem === product.id"
              >
                <span
                  class="spinner-grow spinner-grow-sm"
                  v-show="isLoadingItem === product.id"
                ></span>
                加到購物車</button
              ><!--讀取效果，:disable可避免使用者一直點，造成伺服器 Loading 太重-->
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import emitter from '@/libs/emitter';

export default {
  data() {
    return {
      cartData: {}, // 購物車列表
      products: [], // 產品列表
      isLoadingItem: '',
    };
  },
  methods: {
    getProducts() {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`).then((res) => {
        console.log(res);
        this.products = res.data.products;
      });
    },
    addToCart(id, qty = 1) { // 參數預設值
      const data = {
        product_id: id,
        qty,
      };
      this.isLoadingItem = id; // 觸發時將 Id存取
      this.$http.post(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`, { data })
        .then((res) => {
          console.log(res);
          this.isLoadingItem = '';
          // get-cart
          emitter.emit('get-cart');// 觸發navbar上監聽行為
        });
    },
  },
  mounted() {
    this.getProducts();
  },
};
</script>
