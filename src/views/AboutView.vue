<template>
  <div class="flex-row h-auto">
    <div
      v-if="pokemon"
      class="m-auto bg-red-700 mt-2 flex justify-center flex-col items-center"
    >
      <h3 class="text-5xl text-blue-600 uppercase">{{ pokemon.name }}</h3>
      <div class="flex justify-center">
        <img class="w-48" :src="pokemon.sprites.front_shiny" alt="" />
      </div>
      <ul class="type">
        <h2>Type:</h2>
        <li
          v-for="(type, key) in pokemon.types"
          :key="key"
          :class="type.type.name"
        >
          <span>{{ type.type.name }}</span>
        </li>
      </ul>
      <ul class="stats">
        <h2>Stats:</h2>
        <li v-for="(stat, key) in pokemon.stats" :key="key">
          <span>{{ stat.stat.name }} -> {{ stat.base_stat }}</span>
        </li>
      </ul>
    </div>
    <div class="flex flex-row justify-evenly mt-1 text-3xl bg-yellow-500">
      <button v-on:click="togglefav">
        {{ isFav ? "Favorilerden Kaldır" : "Favorilere Ekle" }}
      </button>
      <button v-on:click="showfav">favorılere gözat</button>
    </div>
  </div>
</template>
<script>
import { useRoute } from "vue-router";
import { ref, computed } from "vue";

export default {
  setup() {
    const route = useRoute();
    const pokemon = ref(null);
    const favorite = ref(JSON.parse(localStorage.getItem("favorite")) ?? []);
    const isFav = computed(() => favorite.value.includes(pokemon.value?.name));
    

    function togglefav() {
      if (!isFav.value) {
        favorite.value.push(pokemon.value.name);
      } else {
        const index = favorite.value.indexOf(pokemon.value.name);
        if (index !== -1) {
          favorite.value.splice(index, 1);
        }
      }

      localStorage.setItem("favorite", JSON.stringify(favorite.value));
    }

    function showfav() {
      alert(localStorage.getItem("favorite"));
    }

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        pokemon.value = data;
      });

    return {
      pokemon,
      togglefav,
      showfav,
      isFav,
    };
  },
};
</script>

<style scoped>
.type {
  display: flex;
  background-color: rgb(228, 149, 212);
  width: 100%;
  justify-content: space-evenly;
}
</style>