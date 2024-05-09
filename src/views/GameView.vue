<template>
  <div>
    <v-container>
      <v-row>
        <v-col cols="6">
          <v-card class="pa-4">
            <v-card-title>Type dates</v-card-title>
            <v-form>
              <v-text-field v-model.number="state.sizeX" label="Размер X" outlined></v-text-field>
              <span v-if="v$.sizeX.$error" style="color: red">
                {{ v$.sizeX.$errors[0].$message }}
              </span>
              <v-text-field v-model.number="state.sizeY" label="Размер Y" outlined></v-text-field>
              <span v-if="v$.sizeY.$error" style="color: red">
                {{ v$.sizeY.$errors[0].$message }}
              </span>
              <v-btn @click="drawSquares" color="primary" block>Draw Squares</v-btn>
            </v-form>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-container class="d-flex justify-center align-center">
      <v-row class="square-body-main" style="background: black">
        <v-col v-for="square in state.squares" :key="square.x * state.sizeY + square.y" cols="auto">
          <div
            class="square"
            :style="{ width: '36px', height: '36px', backgroundColor: square.color }"
            @mouseover="toggleColor(square)"
          ></div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import useValidate from '@vuelidate/core'
import { required, numeric } from '@vuelidate/validators'
import { reactive, computed } from 'vue'

export default {
  setup () {
    const state = reactive({
      sizeX: 0,
      sizeY: 0,
      squares: []
    })
    const rules = computed(() => {
      return {
        sizeX: { required, numeric },
        sizeY: { required, numeric }
      }
    })
    const v$ = useValidate(rules, state)
    return {
      state,
      v$
    }
  },
  methods: {
    drawSquares () {
      this.v$.$validate()
      this.state.squares = []
      for (let i = 0; i < this.state.sizeX; i++) {
        for (let j = 0; j < this.state.sizeY; j++) {
          this.state.squares.push({ x: i, y: j, color: 'white' })
        }
      }
    },
    toggleColor (square) {
      square.color = square.color === 'white' ? 'blue' : 'white'
    }
  }
}
</script>

<style>
.square-body-main {
  background: black;
  max-width: 1200px;
}
.square {
  cursor: pointer;
}
</style>
