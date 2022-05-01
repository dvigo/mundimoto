<template lang="pug">
  v-app
    v-app-bar(
      fixed
      app)
      NuxtLink(to="/")
        Logo
      v-spacer
      v-menu(offset-y)
        template(v-slot:activator="{ on, attrs }")
          v-btn(v-bind="attrs" v-on="on")
            v-icon mdi-account
        v-list
          v-list-item(v-if="!isAuthenticated" href="/login")
            v-list-item-icon
              v-icon mdi-account
            v-list-item-content Acceder
          v-list-item(v-if="!isAuthenticated" href="register")
            v-list-item-icon
              v-icon mdi-account-plus
            v-list-item-content Register
          v-list-item(v-if="isAuthenticated" href="dashboard")
            v-list-item-icon
              v-icon mdi-clipboard-outline
            v-list-item-content Dashboard
          v-list-item(v-if="isAuthenticated" @click="logout")
            v-list-item-icon
              v-icon mdi-run-fast
            v-list-item-content Salir
    v-main
      v-container
        Nuxt
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'default',
  data () {
    return {
      title: 'Home'
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  methods: {
    logout () {
      this.$auth.logout()
    },
  }
}
</script>
