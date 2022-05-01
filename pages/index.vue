<template lang="pug">
  div
    v-row
      v-col(cols="6")
        NuxtLink(to="/tinder")
          v-card
            v-img(src="/logooo.png" height="180px")
      v-col(cols="6")
        NuxtLink(to="/recommendation")
          v-card
            v-img(src="/form.png" height="180px")
    v-container(fluid grid-list-md)
      v-layout(row wrap)
        Product(v-for="product in products" :product="product" :key="product.id")
</template>

<script>
import Product from '~/components/Product.vue'
export default {
  name: 'IndexPage',
  components: {
    Product
  },
  data() {
    return {
      products: []
    }
  },
  async created() {
    await this.$axios.$get('/variant?page=0&page_size=27').then( (res) => this.products = res);
  }
}
</script>

<style>

</style>
