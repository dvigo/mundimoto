<template lang="pug">
v-card
  v-card-title Recommendation BY Artificial Intelligence
    <form @submit.prevent="submitForm">
      v-text-field(type="number", name="model_year", label="Design year")
      v-text-field(type="number", name="displacement", label="CC")
      v-text-field(type="number", label="Top Speed (Km/h)")
      v-text-field(type="number", label="Power (kW)")
      v-text-field(type="number", label="Full capacity")
      v-text-field(type="number", label="Weight (Kg)")
      v-text-field(type="number", label="Valves per cylinder")
      v-select(
        :items="categories",
        label="Category",
        v-model="selected",
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
      console.log(this.form.model_year);
      this.$axios.$get("/variant/recommendation", this.form)
        .then((res) => {
          console.log(res);
        })
        .catch((error) => {
          console.log(error);
          // error.response.status Check status code
        })
        .finally(() => {
          //Perform action in always
        });
    },
  },
};
</script>

<style scoped>
</style>
