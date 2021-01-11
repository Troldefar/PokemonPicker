<template>
  <div class="about">
    <div 
      v-if="pokemon"
      class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col item-center p-10"
    >
      <h3 class="text-2xl text-green-900 uppercase m-auto">
        {{ pokemon.name }}
      </h3>
      <div class="flex justify-center">
        <img class="w-48" :src="pokemon.sprites.front_shiny" :alt="pokemon.name">
        <img class="w-48" :src="pokemon.sprites.back_shiny" :alt="pokemon.name">
      </div>
      <h3 class="m-auto w-100 text-center mt-3">
        Types
      </h3>
      <div class="m-auto" v-for="(type, index) in pokemon.types" :key="index">
        <h3 class="text-blue-900">
          {{ type.type.name }}
        </h3>
      </div>
      <div>
        <h3 class="m-auto w-100 bg-red-100 text-center mt-3">
          Abilities
        </h3>
      </div>
    </div>
    <router-link class="m-auto w-100 btn" to="/">
      Back
    </router-link>
  </div>
</template>

<script>
import { useRoute } from 'vue-router';
import { reactive, toRefs } from 'vue';
export default {
  setup() {
    const route = useRoute();
    
    const state = reactive({
      pokemon: null
    });

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        state.pokemon = data;
      });

    return { ...toRefs(state) }
  }  
}
</script>