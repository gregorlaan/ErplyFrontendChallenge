<template>
  <div>

    <b-container v-if="show" class="single-product">
      <b-row>
        <b-col cols="12" md="4" lg="3">
          <b-img :src="currentProduct[0].image" fluid :alt="currentProduct[0].name" />
        </b-col>
        <b-col cols="12" md="8" lg="9">
          <h1 class="product-name">
            {{ currentProduct[0].name }}
          </h1>
          <p class="price">{{ currentProduct[0].price }}<span><mdi-currency-eur-icon /></span></p>
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
export default {
  data () {
    return {
      productsList: [],
      productId: this.$route.params.productId,
      currentProduct: [],
      show: false
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
    }
  }
}
</script>

<style>
  .single-product .price span svg {
    fill: var(--blue);
  }
  .single-product .price {
    font-size: 2em;
    color: var(--blue);
  }
  .single-product h1.product-name {
    margin-bottom: 20px;
  }
</style>
