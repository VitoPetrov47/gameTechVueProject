<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="pa-4">
          <v-card-title class="text-center">Registration</v-card-title>
          <v-form @submit.prevent="register">
            <v-text-field v-model="state.name" label="Name" outlined required></v-text-field>
            <span v-if="v$.name.$error" style="color: red">
              {{ v$.name.$errors[0].$message }}
            </span>
            <v-text-field v-model="state.email" label="Email" outlined required></v-text-field>
            <span v-if="v$.email.$error" style="color: red">
              {{ v$.email.$errors[0].$message }}
            </span>
            <span v-if="state.existingEmailCheck" style="color: red">
              User with the same email already exists
            </span>
            <v-text-field v-model="state.password.password" label="Password" type="password" outlined required></v-text-field>
            <span v-if="v$.password.password.$error" style="color: red">
              {{ v$.password.password.$errors[0].$message }}
            </span>
            <v-text-field v-model="state.password.confirmPassword" label="Confirm Password" type="password" outlined required></v-text-field>
            <span v-if="v$.password.confirmPassword.$error" style="color: red">
              {{ v$.password.confirmPassword.$errors[0].$message }}
            </span>
            <v-btn type="submit" color="primary" block>Register</v-btn>
          </v-form>
          <v-row justify="center" class="mt-3 pb-3">
            <router-link class="link" to="/login">Log In</router-link>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import useValidate from '@vuelidate/core'
import { required, email, minLength, sameAs } from '@vuelidate/validators'
import { reactive, computed } from 'vue'

export default {
  setup () {
    const state = reactive({
      name: '',
      email: '',
      password: {
        password: '',
        confirmPassword: ''
      },
      existingEmailCheck: false
    })
    const rules = computed(() => {
      return {
        name: { required },
        email: { required, email },
        password: {
          password: { required, minLength: minLength(6) },
          confirmPassword: { required, sameAs: sameAs(state.password.password) }
        }
      }
    })

    const v$ = useValidate(rules, state)

    return {
      state,
      v$
    }
  },
  methods: {
    register () {
      this.v$.$validate()
      if (!this.v$.$error) {
        const existingEmail = localStorage.getItem('email')
        if (existingEmail === this.state.email) {
          this.state.existingEmailCheck = true
          return
        } else {
          this.state.existingEmailCheck = false
        }

        const userId = Date.now()
        const user = {
          id: userId,
          name: this.state.name,
          email: this.state.email,
          password: this.state.password.password
        }

        localStorage.setItem(`user_${userId}`, JSON.stringify(user))

        alert('Success auth')
        this.$router.push('/login')
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
