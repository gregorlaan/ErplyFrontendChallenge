<template>
  <div>product {{ productId }}</div>
</template>

<script>
export default {
  data () {
    return {
      productId: this.$route.params.productId
    }
  }
}
</script>

<style>
</style>
