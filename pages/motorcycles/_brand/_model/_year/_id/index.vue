<template lang="pug">
  v-container(fluid grid-list-md v-if="product.length != 0")
    v-layout(row wrap)
      v-flex(xs12 sm12 md8 class="test")
        div
          v-img(:src="product.image")
        hr
        div
          h1 {{product.brand}} {{product.name}} ({{product.model_year}})
        v-container(fluid grid-list-md)
          v-layout(row wrap)
            v-flex(xs6 sm6 md6 class="test")
              div.non-float
                h4.left Year
                h3.right {{product['model_year']}}
              div.non-float
                h4.left Power
                h3.right {{product['extra_data']['displacement']}}
            v-flex(xs6 sm6 md6 class="test")
              div.non-float
                h4.left Type
                h3.right {{product['extra_data']['category']}}
        div
          h1 More details
        v-container(fluid grid-list-md)
          v-layout(row wrap)
            v-flex(xs6 sm6 md12 class="test")
              div.non-float(v-for="(label, index) in datas" v-if="product['extra_data'][index]")
                h4.left {{label}}
                h3.right {{product['extra_data'][index]}}
      v-flex(xs12 sm12 md4 class="test")
        NuxtLink(to="/tinder")
          v-card.more
            v-img(src="/logooo.png" height="250px")
        NuxtLink(to="/recommendation")
          v-card
            v-img(src="/form-L.png" height="400px")
</template>

<script>
import Form from '~/components/Form.vue'
export default {
  components: {
    Form
  },
  data() {
    return {
      product: [],
      brand: this.$route.params.brand,
      id: this.$route.params.id,
      model: this.$route.params.model,
      year: this.$route.params.year,
      datas: {
        "ignition": "Ignition",
        "chain_links": "Chain Links",
        "compression": "Compression",
        "alternate_seat_height": "Alternate seat height",
        "tire_pressure_rear": "Tire pressure rear",
        "lubrication_system": "Lubrication system",
        "clutch": "Clutch",
        "wheels": "Wheels",
        "exhaust_system": "Exhaust System",
        "oil_filter": "Oil filter",
        "price_as_new": "Price as new",
        "starter": "Starter",
        "fuel_consumption": "Fuel Consumption",
        "front_wheel_travel": "Front wheel travel",
        "category": "Category",
        "top_speed": "Top speed",
        "driveline": "Driveline",
        "overall_height": "Overall height",
        "overall_width": "Overall width",
        "displacement": "Displacement",
        "spark_plugs": "Spark plugs",
        "greenhouse_gases": "Greenhouse gases",
        "instruments": "Instruments",
        "rear_wheel_travel": "Rear wheel travel",
        "battery": "Battery",
        "reserve_fuel_capacity": "Reserve fuel capacity",
        "sprockets": "Sprockets",
        "chain_size": "Chain size",
        "factory_warranty": "Factory warranty",
        "coolant": "Coolant",
        "rear_tire": "Rear tire",
        "rear_suspension": "Rear suspension",
        "seat": "Seat",
        "front_suspension": "Front suspension",
        "rear_brakes": "Rear brakes",
        "electrical": "Electrical",
        "overall_length": "Overall length",
        "bore_x_stroke": "Bore x stroke",
        "weight_incl._oil_gas,_etc": "Weight including oil gas",
        "ground_clearance": "Ground clearance",
        "trail": "Trail",
        "engine_type": "Engine type",
        "0_100_km/h_(0_62_mph)": "0-100 km/h",
        "idle_speed": "Idle speed",
        "color_options": "Color options",
        "comments": "Comments",
        "emission_details": "Emission details",
        "max_rpm": "Max rpm",
        "brake_fluid": "Brake fluid",
        "dry_weight": "Dry weight",
        "front_tire": "Front tire",
        "fuel_system": "Fuel system",
        "valves_per_cylinder": "Valvves per cylinder",
        "year": "Year",
        "tire_pressure_front": "Tire pressure front",
        "engine_details": "Engine details",
        "rake_(fork_angle)": "Rake (fork angle)",
        "oil_capacity": "Oil capacity",
        "frame_type": "Frame type",
        "wheelbase": "Wheelbase",
        "engine_oil": "Engine oil",
        "fuel_control": "Fuel control",
        "seat_height": "Seat height",
        "light": "Light",
        "diameter": "Diameter",
        "fuel_capacity": "Fuel capacity",
        "torque": "Torque",
        "power": "Power",
        "tramsission_type": "Transmission type",
        "front_brakes": "Front brakes",
        "gearbox": "Gearbox",
        "cooling_system": "Cooling system"
      },
    }
  },
  async fetch() {
    await this.$axios.$get('/variant?variant='+this.id+'&page=0&page_size=27').then( (res) => this.product = res);
  },
}
</script>

<style scoped>
.more {
  margin-bottom: 50px;
}
</style>
