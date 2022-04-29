<template>
  <v-app>
    <v-app-bar
      fixed
      app
    >
      <Logo/>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'default',
  data () {
    return {
      title: 'Home'
    }
  }
}
</script>
