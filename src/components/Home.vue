<template>
  <div>
    <b-container>

      <b-form-group label="Select store">
        <b-form-radio-group id="storeState" v-model="storeState" :options="storeOptions" name="storeOptions" @click.native="FilterProducts">
        </b-form-radio-group>
      </b-form-group>

      <b-form-group label="Availability">
        <b-form-radio-group id="availability" v-model="availabilityState" :options="availabilityOptions" name="availabilityOptions" @click.native="FilterProducts">
        </b-form-radio-group>
      </b-form-group>

      <paginate name="products" :list="FilteredProducts" :per="16">
        <li v-bind:key="product.id" v-for="product in paginated('products')">
          <p>{{ product.name }}</p>
          <p>{{ product.store }}</p>
          <p>{{ product.instock }}</p>
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
      paginate: ['products']
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
        console.log(data)
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
    }
  }
}
</script>

<style>
</style>
