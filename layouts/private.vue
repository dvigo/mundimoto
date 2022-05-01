<template lang="pug">
  v-app
    v-app-bar(
      fixed
      app
    )
      NuxtLink(to="/")
        Logo
      v-spacer
      v-menu(offset-y)
        template(v-slot:activator="{ on, attrs }")
          v-btn(v-bind="attrs" v-on="on")
            v-icon mdi-account
            h5(v-if="isAuthenticated") {{ loggedInUser.first_name }} {{ loggedInUser.last_name }}
        v-list
          v-list-item(v-if="!isAuthenticated" href="/login")
            v-list-item-icon
              v-icon mdi-account
            v-list-item-content Acceder
          v-list-item(v-if="!isAuthenticated" href="register")
            v-list-item-icon
              v-icon mdi-account-plus
            v-list-item-content Register
          v-list-item(v-if="isAuthenticated")
            v-list-item-icon
              v-icon mdi-clipboard-outline
            v-list-item-content Dashboard
          v-list-item(v-if="isAuthenticated" @click="logout")
            v-list-item-icon
              v-icon mdi-run-fast
            v-list-item-content Logout
    v-row.all
      v-col(cols="2")
        v-card(
          class=""
          height="95vh"
          width="256"
        )
          v-navigation-drawer(
            class="navbar"
            dark
            permanent
            )
            v-list
              v-list-item(
                v-for="item in items"
                :key="item.title"
                link
              )
                v-list-item-icon
                  v-icon {{ item.icon }}
                v-list-item-content
                  v-list-item-title {{ item.title }}
      v-col(cols="10")
        v-container
          Nuxt
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'private',
  data () {
    return {
      items: [
          { title: 'Dashboard', icon: 'mdi-view-dashboard' },
        ],
      title: 'MundiMoto'
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

<style scoped>
.all {
  padding-top: 75px;
}
.all > .col {
  padding-top: 0px;
  padding-bottom: 0px;
}

.navbar {
  position: fixed;
  padding-top: 80px;
  background: #C1281E;
    background: -webkit-linear-gradient(to top, #1e1e1d, 30%, #C1281E);
  background: linear-gradient(to top, #1e1e1d, 30%, #C1281E);
}
</style>