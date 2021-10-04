<template>
  <div class="about">
    <div
      v-if="pokemon"
      className="sm:w-1/6 m-auto bg-red-400 shadow-2xl flex justify-center flex-col items-center mt-16"
    >
      <h3 className="text-2xl text-black mt-4 uppercase">{{ pokemon.name }}</h3>
      <div class="flex justify-center">
        <img className="w-48" :src="pokemon.sprites.front_default" alt="" />
      </div>
      <p class="text-yellow-300 text-xl">Types</p>
      <div class="mb-4">
        <div v-for="(type, idx) in pokemon.types" :key="idx">
          <li class="text-black capitalize">{{ type.type.name }}</li>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";
import { useRoute } from "vue-router";

export default {
  setup() {
    const route = useRoute();

    const pokemon = reactive({
      pokemon: null,
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        pokemon.pokemon = data;
        console.log(data);
      });
    return { ...toRefs(pokemon) };
  },
};
</script>