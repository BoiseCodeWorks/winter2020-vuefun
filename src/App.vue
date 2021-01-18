<template>
  <div class="text-center">
    <h1>Me <img alt="Vue logo" src="./assets/logo.png" />s the World</h1>
    <form @submit.prevent="addOpponent">
      <!-- V-MODEL TWO WAY DATA BINDING -->
      <input type="text" placeholder="Name" v-model="state.name" />
      <button type="submit" class="btn btn-success">Add</button>
    </form>
    <h3>Foes Remaining: {{ alive.length }}</h3>
    <p v-for="o in alive" :key="o.name">{{ o.name }}</p>
    <div
      class="border p-2 m-2 opponent"
      v-for="opponent in state.opponents"
      :key="opponent.name"
    >
      <p
        :class="{
          healthy: opponent.health > 50,
          hurt: opponent.health <= 50 && opponent.health > 0,
          dead: opponent.health <= 0,
          'bg-danger': opponent.health <= 0,
        }"
        v-for="(value, key) in opponent"
        :key="key"
      >
        {{ key }} : {{ value }}
      </p>
      <p v-if="opponent.health <= 0">GAME OVER</p>

      <button
        type="button"
        class="btn btn-primary"
        :disabled="opponent.health <= 0"
        @click="attack(-1, opponent)"
      >
        Slap
      </button>
      <button
        type="button"
        class="btn btn-warning"
        :disabled="opponent.health <= 0"
        @click="attack(-5, opponent)"
      >
        Punch
      </button>
      <button
        type="button"
        class="btn btn-danger"
        :disabled="opponent.health <= 0"
        @click="attack(-10, opponent)"
      >
        Kick
      </button>

      <button
        type="button"
        class="btn btn-info"
        v-if="!opponent.health <= 0"
        @click="opponent.health = 100"
      >
        Reset
      </button>
      <button
        type="button"
        class="btn btn-info"
        v-else
        @click="opponent.health = 100"
      >
        New Game
      </button>
    </div>
  </div>
</template>


<script>
import { computed, reactive } from 'vue'
export default {
  setup() {
    // NOTE State is for data, AKA ProxyObject (ProxyState)
    const state = reactive({
      name: '',
      // health: 100,
      opponents: [{
        name: 'Mark',
        health: 100,
        weakness: "Spelling"
      }, {
        name: 'Tim',
        health: 100,
        weakness: "Tacos"
      }]
    })
    // all values needed in the template must be returned through this object
    return {
      state,
      // Computeds are used for watching when data changes, if the data changes it will re evaluate
      alive: computed(() => state.opponents.filter(o => o.health > 0)),
      // dead: computed(() => {
      //   return state.health <= 0
      // }),
      attack(val, obj) {
        obj.health += val
        if (obj.health < 0) {
          obj.health = 0
        }
      },
      addOpponent() {
        state.opponents.push({ name: state.name, health: 100 })
        state.name = ''
      }
    }
  }
}
</script>

<style>
h1 {
  color: green;
  text-align: center;
}

.healthy {
  color: green;
}

.hurt {
  color: yellow;
  text-shadow: 1px 1px black;
}

.dead {
  color: red;
}

img {
  height: 1em;
}
</style>