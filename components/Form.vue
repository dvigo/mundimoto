<template lang="pug">
  v-card
    v-card-title Recommendation BY Artificial Intelligence
      v-form
        v-text-field(type="number" label="Design year")
        v-text-field(type="number" label="CC")
        v-text-field(type="number" label="Top Speed (Km/h)" ) // Top Speed KM/h
        v-text-field(type="number" label="Power (kW)")
        v-text-field(type="number" label="Full capacity")
        v-text-field(type="number" label="Weight (Kg)")
        v-text-field(type="number" label="Valves per cylinder")
        v-select(
          :items="categories"
          label="Category"
          v-model="selected"
          @change="changeData")

</template>

<script>
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
        "Prototype / concept model"
      ],
      selected: ''
    }
  },
  methods: {
    changeData() {
      console.log(this.selected);
    }
  }
}
</script>

<style scoped>

</style>
