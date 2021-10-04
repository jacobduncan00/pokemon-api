<template>
  <div>
    <div class="w-full flex justify-center">
      <input
        type="text"
        placeholder="Enter Pokemon Name"
        class="mt-10 p-2 border-red-500 border-2 rounded-md outline-none"
        v-model="userInput"
      />
    </div>
    <div class="mt-10 p-4 flex flex-wrap justify-center">
      <div class="text-xl text-gray-500 italic text-center w-full mb-4">
        Click Pokemon Name Below
      </div>
      <div
        class="ml-4 text-2x text-blue"
        v-for="(pokemon, idx) in filteredPokemon"
        :key="idx"
      >
        <router-link
          :to="`/about/${urlIdLookup[pokemon.name]}`"
          class="
            cursor-pointer
            capitalize
            text-xl
            border-red-500 border-2
            p-2
            rounded
          "
        >
          {{ pokemon.name }}
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from "vue";
export default {
  name: "Home",

  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      userInput: "",
      filteredPokemon: computed(() => updatePokemon()),
    });

    function updatePokemon() {
      if (!state.userInput) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.userInput)
      );
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });
    return { ...toRefs(state) };
  },
};
</script>
