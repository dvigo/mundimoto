<template lang="pug">
  div
    v-card.card-page
      h3 Recommendation BY Artificial Intelligence
      form(@submit.prevent="submitForm")
      v-container
        v-row
          v-col(
            cols="12"
            md="4")
            v-text-field(type="number" name="model_year" label="Design year" v-model="form.model_year")
          v-col(
              cols="12"
              md="4")
            v-text-field(type="number" name="displacement" label="CC" v-model="form.displacement")
          v-col(
              cols="12"
              md="4")
            v-text-field(type="number" label="Top Speed (Km/h)" v-model="form.top_speed")
          v-col(
            cols="12"
            md="4"
          )  
            v-text-field(type="number" label="Power (kW)" v-model="form.power")
          v-col(
            cols="12"
            md="4"
          )   
            v-text-field(type="number" label="Fuel capacity" v-model="form.fuel_capacity")
          v-col(
            cols="12"
            md="4"
          )   
            v-text-field(type="number" label="Weight (Kg)" v-model="form.kg")
          v-col(
            cols="12"
            md="4"
          ) 
            v-text-field(type="number" label="Valves per cylinder" v-model="form.valves_per_cylinder")
          v-col(
            cols="12"
            md="4"
          ) 
            v-select(
              :items="categories",
              label="Category",
              v-model="form.category",
              :v-bind="form.category"
            )
        v-btn(type="submit" color="success" @click="submitForm()") Submit
    v-container(fluid grid-list-md v-if="products != []")
      v-layout(row wrap)
        Product(v-for="product in products" :product="product" :key="product.id")
</template>

<script>
import Product from '~/components/Product.vue'

export default {
  name: "Form",
  components: {
    Product
  },
  data() {
    return {
      form: {},
      products: [],
      categories: [
        "Unspecified category",
        "Classic",
        "Custom / cruiser",
        "Touring",
        "Allround",
        "Naked bike",
        "Sport",
        "Sport touring",
      ],
      selected: "",
    };
  },
  methods: {
    submitForm() {
      console.log('eoo');
      let params = '?';
      for (let filter in this.form) {
        if (params != '?') params += '&';
        if (filter == 'category') {
          this.form[filter] = this.categories.findIndex((element) => element == this.form[filter])
          if(this.form[filter] >= 0) params += filter+'='+this.form[filter]
        } else {
          params += filter+'='+this.form[filter]
        }
      }
      this.$axios.$get("/variant/recommendation"+params)
        .then((res) => {
          this.products = res;
        })
        .catch((error) => {
          
        });
    },
  },
};
</script>

<style scoped>
.card-page {
  padding: 20px;
}
</style>
