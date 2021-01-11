<template>
  <div>
    <div class="w-full flex justify-center">
      <input 
        type="text" 
        placeholder="Search pokedex" 
        class="mt-10 p-2 border-blue-500 border-2"
        v-model="wantedPokemon"
      >
    </div>
    <div class="mt-10 p-4 flex flex-wrap justify-center">
      <div class="ml-4 text-2xl bg-black p-5 text-white mb-2 rounded hover:bg-red-700 cursor-pointer" v-for="(item, index) in filteredPokemon" :key="index">
        <router-link :to="`/about/${urlIdLookup[item.name]}`">
          {{ item.name }}
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from 'vue';
export default {
  name: 'Home',
  setup() {

    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      wantedPokemon: '',
      filteredPokemon: computed(() => updatePokemon())
    });

    function updatePokemon() {
      if(!state.wantedPokemon) {
        return []
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.wantedPokemon)
      );
    }

    fetch('https://pokeapi.co/api/v2/pokemon?offset=0')
      .then((res) => res.json())
      .then((data) => {
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce((acc, curr, index) =>
          acc = { ...acc, [curr.name] : index + 1 }, {})
    });

    return { ...toRefs(state) }

  }
}
</script>
