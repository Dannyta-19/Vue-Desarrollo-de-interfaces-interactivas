<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";


export default {
  name: "App",
  data() {
    return {
      pokemones: [],
    };
  },
  methods: {
    async getPokemones() {
      try {
        const url = "https://pokeapi.co/api/v2/pokemon";
        const { data: { results: pokemones } } = await axios.get(url);

        pokemones.forEach(async ({ url }) => {
          const { data } = await axios.get(url);
          const {
            name,
            sprites: {
              other: {
                dream_world: { front_default },
              },
            },
          } = data;

          this.pokemones.push({ name, url: front_default, descubierto: false });
        });
      } catch (error) {
        console.error("Error al obtener los pokemones:", error);
      }
    },
    descubrirPokemon({ inputName, index }) {
      if (this.pokemones[index].name.toLowerCase() == inputName.toLowerCase())
        this.pokemones[index].descubierto = true;
      else alert("Incorrecto");
    },
  },
  computed: {
    pokemonesDescubiertos() {
      return this.pokemones.filter((p) => !!p.descubierto).length;
    },
  },
  mounted() {
    this.getPokemones();
  },
  components: {
    PokemonCard,
  },
};
</script>


<template>
  <div id="app">
    <img height="100" src="/pokemon.webp" />
    <h1>¿Quién es ese Pokémon?</h1>
    <h3>
      Pokemones descubiertos: <span>{{ pokemonesDescubiertos }}</span>
    </h3>
    <div class="grid">
      <div class="card" v-for="(pokemon, i) in pokemones" :key="i">
        <PokemonCard
          :pokemon="pokemon"
          @descubrirPokemon="descubrirPokemon"
          :index="i"
        />
      </div>
    </div>
  </div>
</template>


<style lang="scss">
#app {
  padding: 20px;
  text-align: center;
  font-family: sans-serif;
}

.grid {
  margin-top: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

h3 {
  span {
    color: #ffcb03;
    text-shadow: 0px 0px 2px #095a9a;
  }
}
</style>
