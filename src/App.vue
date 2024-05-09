<template>
  <v-app>
    <v-main>
      <div v-if="loader" class="loader-overlay">
        <v-progress-circular
          color="primary"
          indeterminate
        ></v-progress-circular>
      </div>
      <template v-if="!loader">
        <template v-if="showNavBar">
          <NavBar/>
        </template>
        <router-view/>
      </template>
    </v-main>
  </v-app>
</template>

<style>
.loader-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.5); /* прозрачный белый цвет */
}
</style>

<script>
import NavBar from '@/components/NavBar.vue'

export default {
  name: 'App',
  components: { NavBar },
  data: () => ({
    loader: true
  }),
  mounted () {
    setTimeout(() => {
      this.loader = false
    }, 500)
  },
  computed: {
    showNavBar () {
      return this.$route.meta.showNavBar !== false
    }
  },
  beforeRouteEnter (to, from, next) {
    next(vm => {
      vm.loader = true
    })
  }
}
</script>
