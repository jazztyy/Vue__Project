<template>
  <div>
    <layout-header ref="cartModal"/>
    <div class="banner"></div>
    <main class="products">
      <div class="container">
        <layout-aside></layout-aside>
        <ul class="product__list">
          <li v-for="product of products" class="product" :key="product.id">
            <div>
              <img :src="product.imageUrl[0]" alt />
              <div class="product__info">
                <div class="product__title">
                  <h4>{{ product.title }}</h4>
                  <span>{{ product.category }}</span>
                </div>
                <div class="product__price">
                  <span>{{ product.origin_price }}</span>
                  <span>{{ product.price }}</span>
                </div>
              </div>
              <div class="product__btn">
                <button>產品內容</button>
                <button @click="addtoCart(product)">加入購物車</button>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </main>
    <layout-footer/>
  </div>
</template>

<script>

import LayoutHeader from '../components/LayoutHeader'
import LayoutAside from '../components/LayoutAside'
import LayoutFooter from '../components/LayoutFooter'

export default {
  name: 'ProductList',
  components: {
    LayoutHeader,
    LayoutAside,
    LayoutFooter
  },
  data () {
    return {
      isLoading: false,
      products: [],
      pagination: {},
      tempProduct: {
        imageUrl: []
      },
      cart: {},
      cartupon: 0,
      coupon: {},
      coupon_code: ''
    }
  },
  created () {
    this.getProductList()
  },
  methods: {
    loading (status) {
      this.isLoading = status
    },
    getProductList (page = 1) {
      const api = `${process.env.VUE_APP_APIPATH}${process.env.VUE_APP_UUID}/ec/products?page=${page}`
      this.isLoading = true
      this.axios.get(api)
        .then((res) => {
          this.pagination = res.data.meta.pagination
          this.products = res.data.data
          this.isLoading = false
        })
    },
    addtoCart (item, quantity = 1) {
      const url = `${process.env.VUE_APP_APIPATH}${process.env.VUE_APP_UUID}/ec/shopping`
      const cart = {
        product: item.id,
        quantity
      }
      this.isLoading = true
      this.axios.post(url, cart).then((res) => {
        this.$refs.cartModal.getCart()
      }).catch((error) => {
        this.isLoading = false
        console.log(error)
      })
    }
  }
}
</script>
