<template>
  <div>

    <b-container>
      <b-row>
        <div v-if="show">
          {{ currentProduct[0].name }}
        </div>
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
</style>
