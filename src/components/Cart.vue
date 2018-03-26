<template>
  <div>
    <b-container class="cart">
      <b-row>
        <b-col>

          <b-list-group>
            <b-list-group-item v-bind:key="product" v-for="product in localCart">
              {{ product }}
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
      localCart: []
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
    }
  }
}
</script>

<style>
</style>
