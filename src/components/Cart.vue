<template>
  <div>
    <b-container v-if="show" class="cart">
      <b-row>
        <b-col>

          <b-list-group>
            <b-list-group-item v-bind:key="key" v-for="(quantity, key) in groupedLocalCart">
              {{ key }} <b-badge variant="primary" pill>{{ quantity }}</b-badge>
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
      groupedLocalCart: {},
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
        this.getFromCart(data)
        this.show = true
      }, function (response) {
        // error callback
      })
    },
    getFromCart: function (data) {
      // get strings from localStorage
      var strings = Vue.localStorage.get('cart')
      // strings from localStorage to array
      this.localCart = strings.split(',')
      var groupProducts = {}
      this.localCart.forEach(function (x) {
        groupProducts[data[x - 1].name] = (groupProducts[data[x - 1].name] || 0) + 1
      })
      this.groupedLocalCart = groupProducts
    }
  }
}
</script>

<style>
</style>
