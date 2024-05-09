
<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="pa-4">
          <v-card-title class="text-center">Log In</v-card-title>
          <v-form @submit.prevent="login">
            <v-text-field v-model="state.email" label="Email" outlined required></v-text-field>
            <span v-if="v$.email.$error" style="color: red">
              {{ v$.email.$errors[0].$message }}
            </span>
            <v-text-field v-model="state.password" label="Пароль" type="password" outlined required></v-text-field>
            <span v-if="v$.password.$error" style="color: red">
              {{ v$.password.$errors[0].$message }}
            </span>
            <v-btn type="submit" color="primary" block>Log In</v-btn>
          </v-form>
          <v-row justify="center" class="mt-3 pb-3">
            <router-link class="link" to="/register">Register now</router-link>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import useValidate from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
import { reactive, computed } from 'vue'

export default {
  setup () {
    const state = reactive({
      email: '',
      password: ''
    })
    const rules = computed(() => {
      return {
        email: { required, email },
        password: { required }
      }
    })
    const v$ = useValidate(rules, state)
    return {
      state,
      v$
    }
  },
  methods: {
    login () {
      this.v$.$validate()
      if (!this.v$.$error) {
        const storedEmail = localStorage.getItem('email')
        const storedPassword = localStorage.getItem('password')
        if (this.state.email === storedEmail && this.state.password === storedPassword) {
          localStorage.setItem('authenticated', true)
          this.$router.push('/game')
        } else {
          alert('Неправильный логин или пароль')
        }
      }
    }
  }
}
</script>

<style>
.link {
  text-decoration: none;
  color: #089264;
}
</style>
