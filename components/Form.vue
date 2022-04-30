<template lang="pug">
v-card.card-page
    h3 Recommendation BY Artificial Intelligence
    <form @submit.prevent="submitForm">
      v-text-field(type="number" name="model_year" label="Design year" v-model="form.model_year")
      v-text-field(type="number" name="displacement" label="CC" v-model="form.displacement")
      v-text-field(type="number" label="Top Speed (Km/h)" v-model="form.top_speed")
      v-text-field(type="number" label="Power (kW)" v-model="form.power")
      v-text-field(type="number" label="Fuel capacity" v-model="form.fuel_capacity")
      v-text-field(type="number" label="Weight (Kg)" v-model="form.kg")
      v-text-field(type="number" label="Valves per cylinder" v-model="form.valves_per_cylinder")
      v-select(
        :items="categories",
        label="Category",
        v-model="form.category",
        :v-bind="form.category"
      )
      <v-btn type="submit" color="success">Submit</v-btn>
    </form>
</template>

<script>
import axios from 'axios';

export default {
  name: "Form",
  data() {
    return {
      form: {},
      categories: [
        "Unspecified category",
        "Allround",
        "Super motard",
        "Minibike, cross",
        "Cross / motocross",
        "Enduro / offroad",
        "Trial",
        "Touring",
        "Classic",
        "Custom / cruiser",
        "Scooter",
        "Naked bike",
        "Sport",
        "Sport touring",
        "Speedway",
        "Prototype / concept model",
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
          console.log(res);
        }).finally(() => {
          //Perform action in always
        })
        .catch((error) => {
          console.log(error);
          // error.response.status Check status code
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
