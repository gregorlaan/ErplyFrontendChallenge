<template>

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

</template>

<script>
export default {
}
</script>

<style>
</style>
