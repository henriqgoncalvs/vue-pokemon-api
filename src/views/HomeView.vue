<template>
  <div class="w-full flex justify-center">
    <input aria-label="pokemon" type="text" placeholder="Enter Pokemon here"
      class="mt-10 p-2 border-blue-500 border-2"
      v-model="pokemonName"
     />
  </div>

  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div class="ml-4 text-2xl text-blue-400"
      v-for="(pokemon, index) in filteredPokemons"
      :key="index"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from 'vue';

export default {
  name: 'HomeView',
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {

      },
      pokemonName: '',
      // eslint-disable-next-line no-use-before-define
      filteredPokemons: computed(() => updatePokemon()),
    });

    const updatePokemon = () => {
      if (!state.pokemonName) return state.pokemons;

      return state.pokemons.filter((pokemon) => pokemon.name.includes(state.pokemonName));
    };

    fetch('https://pokeapi.co/api/v2/pokemon')
      .then((res) => res.json())
      .then((data) => {
        const { results } = data;

        state.pokemons = results;
        state.urlIdLookup = results.reduce(
          (acc, cur, index) => ({ ...acc, [cur.name]: index + 1 }),
          {},
        );
      });

    return {
      ...toRefs(state),
    };
  },
};
</script>
