<template>
  <div>
    <b-container>

      <paginate name="products" :list="productsList" :per="16">
        <li v-bind:key="product.id" v-for="product in paginated('products')">
          {{ product.name }}
        </li>
      </paginate>

      <paginate-links for="products"></paginate-links>

    </b-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      productsList: {},
      paginate: ['products']
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
        console.log(data)
      }, function (response) {
        // error callback
      })
    }
  }
}
</script>

<style>
</style>
