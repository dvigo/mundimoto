<template lang="pug">
  v-flex(xs12 sm6 md4 class="test")
    NuxtLink(:to="productUrl" style="text-decoration: none; color: inherit;")
      v-card(max-height="400px" min-height="330px" )
        v-card-title(primary class="title") {{product.name}} ({{product.model_year}})
        v-card-text
          v-img(:src="product.image" height="200px")
        v-card-text(class="text-left price") {{product.price}} €
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    }
  },
  computed: {
    productUrl() {
      return (this.product != "") ? '/motorcycles/' + encodeURIComponent(this.product.brand) + '/' + encodeURIComponent(this.product.name) + '/' + this.product.model_year + '/' + this.product.id : '';
    }
  }
}
</script>

<style scoped>
.price {
  font-size: 18px;
  color: black;
}
</style>
