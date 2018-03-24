<template>
  <div>
    <b-container>
      <li v-bind:key="product.id" v-for="product in products">
        {{ product.name }}
      </li>
    </b-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products: {}
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
        this.products = data
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
