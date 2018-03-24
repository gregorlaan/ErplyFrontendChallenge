<template>
  <div>
    <b-container>

      <b-form-group label="Select store">
        <b-form-radio-group id="storeState" v-model="storeState" :options="options" name="storeOptions" @click.native="updateFilterState">
        </b-form-radio-group>
      </b-form-group>

      <paginate name="products" :list="FilteredStore" :per="16">
        <li v-bind:key="product.id" v-for="product in paginated('products')">
          <p>{{ product.name }}</p>
          <p>{{ product.store }}</p>
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
      FilteredStore: [],
      storeState: 'All',
      options: [
        { text: 'All', value: 'All' },
        { text: 'Estonia', value: 'Estonia' },
        { text: 'Finland', value: 'Finland' }
      ],
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
        this.filterStore(data)
      }, function (response) {
        // error callback
      })
    },
    filterStore: function (data) {
      this.FilteredStore = []
      for (var key in data) {
        if (data[key].store === this.storeState) {
          this.FilteredStore.push(data[key])
        }
        if (this.storeState === 'All') {
          this.FilteredStore = data
        }
      }
    },
    updateFilterState: function () {
      this.getData()
    }
  }
}
</script>

<style>
</style>
