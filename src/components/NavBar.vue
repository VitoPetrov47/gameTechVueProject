<template>

  <v-app>
    <v-toolbar app>
      <v-toolbar-title>
        <router-link class="main-link" to="/" style="cursor: pointer">
          {{ appTitle }}
        </router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn
          flat
          v-for="item in menuItems"
          :key="item.title"
          :to="item.path">
          <v-icon left dark>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-btn>
        <v-btn outlined class="ml-3" @click="logout">
          Log Out
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <v-content>
      <router-view></router-view>
    </v-content>

  </v-app>

</template>

<script>

export default {
  name: 'NavBar',
  data () {
    return {
      appTitle: 'Tech Game',
      sidebar: false,
      menuItems: [
        { title: 'About', path: '/about', icon: 'home' },
        { title: 'Game', path: '/game', icon: 'face' }
      ]
    }
  },
  methods: {
    logout () {
      localStorage.removeItem('authenticated')
      this.$router.push('/login')
    }
  }
}
</script>

<style>
.main-link {
  text-decoration: none;
  cursor: pointer;
  font-weight: 600;
  color: black;
}
</style>
