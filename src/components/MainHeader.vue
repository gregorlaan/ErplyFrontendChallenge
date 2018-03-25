<template>

  <div>

    <b-navbar toggleable="md" type="dark" variant="primary">

    <b-container>

      <b-navbar-brand href="#">E-shop SPA</b-navbar-brand>

      <b-navbar-toggle target="nav_collapse" class="ml-auto"></b-navbar-toggle>

      <b-collapse is-nav id="nav_collapse">

        <b-navbar-nav class="ml-auto">
          <b-nav-item to="/">Home</b-nav-item>
          <b-nav-item to="cart">Cart</b-nav-item>
        </b-navbar-nav>

      </b-collapse>

    </b-container>

    </b-navbar>

    <b-container>
      <b-breadcrumb :items="items"/>
    </b-container>

  </div>

</template>

<script>
export default {
  data () {
    return {
      items: [{
        text: 'Home',
        to: '/',
        active: true
      }]
    }
  }
}
</script>

<style>
  .breadcrumb {
    margin-top: 25px;
  }
</style>
