<template>
  <div>
    <b-container class="cart">
      <b-row>
        <b-col>

          <b-list-group>
            <b-list-group-item v-bind:key="key" v-for="(product, key) in groupedLocalCart">
              {{ key }} - {{ product }}
            </b-list-group-item>
          </b-list-group>

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
      localCart: [],
      groupedLocalCart: {}
    }
  },
  mounted: function () {
    this.getFromCart()
  },
  methods: {
    getFromCart: function () {
      // get strings from localStorage
      var strings = Vue.localStorage.get('cart')
      // strings from localStorage to array
      this.localCart = strings.split(',')
      var groupProducts = {}
      this.localCart.forEach(function (x) {
        groupProducts[x] = (groupProducts[x] || 0) + 1
      })
      this.groupedLocalCart = groupProducts
    }
  }
}
</script>

<style>
</style>
