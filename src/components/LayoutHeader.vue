<template>
  <header>
    <div class="cart__list" v-show="isShow">
      <h3>購物車清單</h3>
      <i class="fas fa-times" @click="isShow = false"></i>
      <ul>
        <li v-for="product of cart" class="cart__list__product" :key="product.id">
          <div class="cart__info">
            <h4>{{ product.product.title }}</h4>
            <img :src="product.product.imageUrl[0]" alt />
            <p>{{ product.product.price }} 元</p>
            <div>
              <button>
                <i
                  class="fas fa-minus"
                  @click="changeProductNum(product.quantity - 1, product.product.id)"
                ></i>
              </button>
              <input type="text" disabled :value="product.quantity" />
              <button>
                <i
                  class="fas fa-plus"
                  @click="changeProductNum(product.quantity + 1, product.product.id)"
                ></i>
              </button>
            </div>
            <i class="fas fa-times" @click="removeCartProduct(product.product.id)"></i>
          </div>
        </li>
      </ul>
      <div class="cart__detail">
        <div class="cart__coupons">
          <div>
            <span>優惠券：</span>
            <input type="text" placeholder="請輸入優惠券" />
            <i class="far fa-check-circle useful" v-show="isUseful"></i>
            <i class="fas fa-exclamation-circle unuseful" v-show="isUnuseful"></i>
          </div>
          <span>折抵了 {{ }} 元</span>
        </div>
        <div class="cart__total">
          <button @click="removeAllCartProduct">刪除全品項</button>
          <p>
            總計：
            <span>{{ cartTotal }}</span>
          </p>
        </div>
        <div class="cart__btn">
          <button @click="isVerifyShow = true, isShow = false">結帳</button>
        </div>
      </div>
    </div>
    <nav>
      <ul>
        <li>
          <a href>
            <i class="fas fa-gift"></i>
            <router-link to='/products'><span>產品列表</span></router-link>
          </a>
        </li>
        <li>
          <a href>
            <i class="far fa-calendar-alt"></i>
            <span>活動資訊</span>
          </a>
        </li>
      </ul>
      <router-link to='/'><h1>原型實物</h1></router-link>
      <ul>
        <li>
          <a href>
            <i class="fas fa-user"></i>
            <span>會員</span>
          </a>
        </li>
        <li>
          <a
            href="#"
            @click.prevent="isCartShow = !isCartShow, isShow = false, isVerifyShow = false"
          >
            <i class="fas fa-shopping-cart"></i>
            <span>購物車</span>
            <div class="cart__num" v-show="cart[0]">
              <span>{{ cart.length }}</span>
            </div>
          </a>
          <div class="cart" v-show="isCartShow">
            <ul>
              <p v-show="!cart[0]">購物車現在很孤單，加入商品陪伴他吧</p>
              <li v-for="product of cart" :key="product.product.id">
                <img :src="product.product.imageUrl[0]" alt />
                <div class="cart__info">
                  <h4>{{ product.product.title }}</h4>
                  <p>{{ product.product.price }} 元</p>
                  <div>
                    <button>
                      <i
                        class="fas fa-minus"
                        @click="changeProductNum(product.quantity - 1, product.product.id)"
                      ></i>
                    </button>
                    <input type="text" disabled :value="product.quantity" />
                    <button>
                      <i
                        class="fas fa-plus"
                        @click="changeProductNum(product.quantity + 1, product.product.id)"
                      ></i>
                    </button>
                  </div>
                  <i class="fas fa-times" @click="removeCartProduct(product.product.id)"></i>
                </div>
              </li>
            </ul>
            <div class="cart__detail">
              <div class="cart__total">
                <button @click="removeAllCartProduct">刪除全品項</button>
                <p>
                  總計：
                  <span>{{ cartTotal }}</span>
                </p>
              </div>
              <div class="cart__btn">
                <button @click="isShow = true, isCartShow = false">查看購物車</button>
                <button @click="isVerifyShow = true, isCartShow = false">結帳</button>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'LayoutHeader',
  data () {
    return {
      cart: {},
      cartTotal: 0,
      isCartShow: false,
      isUseful: false,
      isUnuseful: false,
      isShow: false,
      isVerifyShow: false,
      form: {
        name: '',
        email: '',
        phone: '',
        address: '',
        payment: ''
      }
    }
  },
  created () {
    this.getCart()
  },
  methods: {
    getCart () {
      const url = `${process.env.VUE_APP_APIPATH}${process.env.VUE_APP_UUID}/ec/shopping`
      this.axios.get(url).then((response) => {
        this.cartTotal = 0
        this.cart = response.data.data
        this.cart.forEach(product => {
          this.cartTotal += (product.product.price * product.quantity)
        })
        this.loading(false)
      }).catch(error => {
        this.loading(false)
        console.log(error)
      })
    },
    removeCartProduct (id) {
      this.loading(true)
      const url = `${process.env.VUE_APP_APIPATH}${process.env.VUE_APP_UUID}/ec/shopping/${id}`
      this.axios.delete(url).then(() => {
        this.getCart()
      })
    },
    removeAllCartProduct () {
      this.loading(true)
      const url = `${process.env.VUE_APP_APIPATH}${process.env.VUE_APP_UUID}/ec/shopping/all/product`
      this.tempProduct = []
      this.axios.delete(url)
        .then(() => {
          this.getCart()
        })
    },
    changeProductNum (num, id) {
      if (num === 0) {
        this.removeCartProduct(id)
      } else {
        this.loading(true)
        const url = `${process.env.VUE_APP_APIPATH}${process.env.VUE_APP_UUID}/ec/shopping`
        const data = {
          product: id,
          quantity: num
        }
        this.axios.patch(url, data).then(() => {
          this.getCart()
        })
      }
    },
    loading (status) {
      this.$emit('loading', status)
    }
  }
}
</script>
