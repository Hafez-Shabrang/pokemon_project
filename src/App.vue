<template>
  <div class="container">
    <div class="row jusify-content-center my-3">
      <div class="col-md-4 mt-5 mb-3" v-for="starter in starters">
        <div class="card text-center starter-card"  @click="fetchEvolutions(starter)">
          <ul class="list-group list-group-flush">
            <li class="list-group-item">{{ starter.name }}</li>
            <li class="list-group-item"><img :src="starter.sprite" :alt="starter.name"></li>
            <li class="list-group-item">
              <div v-for="t in starter.types">{{ t }}</div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="row jusify-content-center my-3">
      <h1 class="text-center mt-5 mb-2">Evolutions</h1>
      <div class="col-md-4 mt-5 mb-3" v-for="creature in evolutions">
        <div class="card text-center">
          <ul class="list-group list-group-flush">
            <li class="list-group-item">{{ creature.name }}</li>
            <li class="list-group-item"><img :src="creature.sprite" :alt="creature.name"></li>
            <li class="list-group-item">
              <div v-for="t in creature.types">{{ t }}</div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    
  </div>
</template>


<script>
import { RouterLink, RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue';
const api = "https://pokeapi.co/api/v2/pokemon"
const STARTER_ID = [1,4,7]

export default {
  components: {
    Navbar
  },

  data() {
    return {
      starters: [],
      evolutions: []
    }
  },

  async created() {
    const starters = await this.fetchData(STARTER_ID)
    this.starters = starters
  },

  methods: {

    async fetchEvolutions(pokemon) {
      this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2])
    },
     
    async fetchData(ids) {
      const responses = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)))
      const data = await Promise.all(responses.map(res => res.json()))
      return data.map(datum => ({
        id: datum.id,
        name: datum.name,
        sprite: datum.sprites.other["official-artwork"].front_default,
        types: datum.types.map(type => type.type.name)
      }))
    }
  }
}
</script>


<style scoped>
img {
  width: 200px;
  padding: 30px 0;
}
.starter-card:hover{
  cursor: pointer;
}
</style>
