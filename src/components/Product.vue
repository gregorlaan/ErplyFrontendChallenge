<template>
  <div>

    <b-container v-if="show" class="single-product">
      <b-row>

        <b-col cols="12">
          <b-alert class="cart-message" v-if="cartMessage" show>
            Product added to the cart!
            <button type="button" aria-label="Close" class="close" v-on:click="cartMessage = false">×</button>
          </b-alert>
        </b-col>

        <b-col cols="12" md="4" lg="3">
          <div class="image-wrapper">
            <b-img :src="currentProduct[0].image" fluid :alt="currentProduct[0].name" />
          </div>
        </b-col>

        <b-col cols="12" md="8" lg="9">
          <h1 class="product-name">
            {{ currentProduct[0].name }}
          </h1>
          <p>
            Product code: {{ currentProduct[0].productcode }}
          </p>
          <p class="price">{{ currentProduct[0].price }}<span><mdi-currency-eur-icon /></span></p>
          <p class="availability" v-bind:class="{ 'in-stock': currentProduct[0].instock, 'out-of-stock': !currentProduct[0].instock }">
            <mdi-checkbox-marked-circle-outline-icon />
            <span v-if="currentProduct[0].instock">In Stock</span>
            <span v-else>Out of Stock</span>
          </p>
          <b-button v-on:click="addToCart()" variant="primary" class="add-to-cart" hrevariant="primary">Add To Cart <mdi-cart-outline-icon /></b-button>
          <p class="desc">
            {{ currentProduct[0].description }}
          </p>
          <p class="category"><mdi-tag-multiple-icon /> {{ currentProduct[0].department }}</p>
          <p class="country" title="country"><mdi-earth-icon /> {{ currentProduct[0].store }}</p>
        </b-col>

      </b-row>
    </b-container>

  </div>
</template>

<script>
import Vue from 'vue'

export default {
  data () {
    return {
      productsList: [],
      productId: this.$route.params.productId,
      currentProduct: [],
      show: false,
      cartMessage: false
    }
  },
  mounted: function () {
    this.getData()
  },
  methods: {
    getData: function () {
      this.$http({
        url: 'https://erply-challenge.herokuapp.com/list?AUTH=fae7b9f6-6363-45a1-a9c9-3def2dae206d',
        method: 'GET'
      }).then(function (response) {
        // success callback
        var data = response.body
        this.productsList = data
        this.currentProduct = []
        for (var key in this.productsList) {
          if (key === this.$route.params.productId) {
            this.currentProduct.push(this.productsList[key - 1])
          }
        }
        this.show = true
      }, function (response) {
        // error callback
      })
    },
    addToCart: function () {
      var localCart = []
      if (Vue.localStorage.get('cart')) {
        localCart.push(Vue.localStorage.get('cart'))
      }
      localCart.push(this.productId)
      Vue.localStorage.set('cart', localCart)
      this.cartMessage = true
    }
  }
}
</script>

<style>
  .single-product .image-wrapper {
    text-align: center;
    margin: 5px 0 20px;
  }
  .single-product .price span svg {
    fill: var(--blue);
  }
  .single-product .price {
    font-size: 2em;
    color: var(--blue);
  }
  .single-product .add-to-cart {
    margin-bottom: 20px;
  }
  .single-product .add-to-cart svg {
    fill: white;
  }
  .single-product .availability.out-of-stock svg {
    fill: var(--danger);
  }
  .single-product .availability.in-stock svg {
    fill: var(--success);
  }
  .cart-message button.close {
    position: absolute;
    top: 0;
    right: 0;
    padding: .75rem 1rem;
    color: inherit;
  }
</style>
