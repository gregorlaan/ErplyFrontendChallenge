<template>
  <div>

    <b-container>
      <b-row>

      <b-col cols="12">
        <b-alert class="cart-message" v-if="cartMessage" show>
          Product added to the cart!
          <button type="button" aria-label="Close" class="close" v-on:click="cartMessage = false">×</button>
        </b-alert>
      </b-col>

      <b-col cols="12">
        <div class="filters">

          <b-form-group label="Select store">
            <b-form-radio-group id="storeState" v-model="storeState" :options="storeOptions" name="storeOptions">
            </b-form-radio-group>
          </b-form-group>

          <b-form-group label="Availability">
            <b-form-radio-group id="availability" v-model="availabilityState" :options="availabilityOptions" name="availabilityOptions">
            </b-form-radio-group>
          </b-form-group>

        </div>
      </b-col>

      <paginate class="products-list" name="products" :list="FilteredProducts" :per="16">
        <b-col cols="12" sm="6" md="4" xl="3" v-bind:key="product.id" v-for="product in paginated('products')">
          <b-card
          tag="article"
          class="mb-2">
            <div class="image-wrapper">
              <b-img :src="product.image" fluid :alt="product.name" />
            </div>
            <b-link :to="'/product/' + product.id"><h2 class="product-name">{{ product.name }}</h2></b-link>
            <p class="country" title="country"><mdi-earth-icon /> {{ product.store }}</p>
            <p class="category"><mdi-tag-multiple-icon /> {{ product.department }}</p>
            <p class="availability" v-bind:class="{ 'in-stock': product.instock, 'out-of-stock': !product.instock }">
              <mdi-checkbox-marked-circle-outline-icon />
              <span v-if="product.instock">In Stock</span>
              <span v-else>Out of Stock</span>
            </p>
            <p class="price">{{ product.price }}<span><mdi-currency-eur-icon /></span></p>
            <b-button v-on:click="addToCart(product.id)" class="add-to-cart" variant="primary">Add To Cart <mdi-cart-outline-icon /></b-button>
          </b-card>
        </b-col>
      </paginate>

      <b-col cols="12">
        <paginate-links for="products"
        :simple="{
          prev: 'Back',
          next: 'Next'
        }"
        :classes="{
          'ul': ['btn-group', 'btn-group-md'],
          '.next': ['btn', 'btn-outline-primary'],
          '.prev': ['btn', 'btn-outline-primary'] // multiple classes
        }"
        ></paginate-links>
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
      FilteredProducts: [],
      storeState: 'All',
      availabilityState: 'All',
      storeOptions: [
        { text: 'All', value: 'All' },
        { text: 'Estonia', value: 'Estonia' },
        { text: 'Finland', value: 'Finland' }
      ],
      availabilityOptions: [
        { text: 'All', value: 'All' },
        { text: 'In Stock', value: true },
        { text: 'Out of Stock', value: false }
      ],
      paginate: ['products'],
      cartMessage: false
    }
  },
  // run FilterProducts function if there are made changes to storeState or availabilityState
  watch: {
    storeState: function () {
      this.FilterProducts()
    },
    availabilityState: function () {
      this.FilterProducts()
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
        this.FilterProducts()
      }, function (response) {
        // error callback
      })
    },
    FilterProducts: function () {
      this.FilteredProducts = [] // Reset array
      for (var key in this.productsList) {
        if (this.productsList[key].store === this.storeState && this.productsList[key].instock === this.availabilityState) {
          this.FilteredProducts.push(this.productsList[key])
        }
        if (this.storeState === 'All' && this.availabilityState === 'All') {
          this.FilteredProducts = this.productsList
        }
        if (this.availabilityState === 'All' && this.productsList[key].store === this.storeState) {
          this.FilteredProducts.push(this.productsList[key])
        }
        if (this.storeState === 'All' && this.productsList[key].instock === this.availabilityState) {
          this.FilteredProducts.push(this.productsList[key])
        }
      }
    },
    addToCart: function (id) {
      var localCart = []
      if (Vue.localStorage.get('cart')) {
        localCart.push(Vue.localStorage.get('cart'))
      }
      localCart.push(id)
      Vue.localStorage.set('cart', localCart)
      this.cartMessage = true
    }
  }
}
</script>

<style>
  ul.products-list {
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    padding: 0;
  }
  ul.products-list .image-wrapper {
    background: whitesmoke;
    height: 150px;
    overflow: hidden;
    display: flex;
    width: 100%;
    justify-content: center;
    align-items: center;
  }
  ul.products-list .image-wrapper img {
    width: 100%;
  }
  ul.products-list article {
    height: calc(100% - 20px);
    transition: all 200ms ease;
  }
  ul.products-list article:hover {
    border-color: rgb(0, 122, 255);
  }
  ul.products-list article .add-to-cart {
    width: 100%;
  }
  ul.products-list article .card-body {
    margin-bottom: 10px;
  }
  ul.products-list article .price span svg {
    fill: var(--blue);
  }
  ul.products-list article .price {
    font-size: 2em;
    text-align: center;
    color: var(--blue);
  }
  ul.products-list article .availability.out-of-stock svg {
    fill: var(--danger);
  }
  ul.products-list article .availability.in-stock svg {
    fill: var(--success);
  }
  ul.products-list article h2.product-name {
    font-size: 24px;
    height: 84px;
    margin-top: 15px;
  }
  .filters {
    padding: 2rem;
    margin-bottom: 1rem;
    border-style: solid;
    border-width: 1px;
    border-color: var(--primary);
    border-radius: 0.3rem;
  }
  ul.paginate-links {
    display: flex;
    justify-content: center;
    padding: 0;
  }
  ul.products-list article .add-to-cart svg {
    fill: white;
  }
</style>
