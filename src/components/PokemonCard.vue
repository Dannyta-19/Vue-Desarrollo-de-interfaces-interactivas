<template>
    <div class="pokemon-card">
      <!-- Imagen del Pokémon -->
      <img
        height="100"
        :src="pokemon.url"
        :class="{ filtered: !pokemon.descubierto }"
        alt="Imagen del Pokémon"
      />
      
      <!-- Nombre del Pokémon  -->
      <span v-show="pokemon.descubierto"> {{ pokemon.name }}</span>
      
      
      <input
        v-show="!pokemon.descubierto"
        v-model="inputName"
        @keypress.enter="intentarDescubrir"
        placeholder="Ingresa el nombre"
      />
      
      <!-- Botón para descubrir el Pokémon -->
      <button
        v-show="!pokemon.descubierto"
        @click="intentarDescubrir"
      >
        Descubrir
      </button>
    </div>
  </template>
  
  <script>
  export default {
    name: "PokemonCard",
    props: {
      pokemon: {
        type: Object,
        required: true,
      },
      index: {
        type: Number,
        required: true,
      },
    },
    data() {
      return {
        inputName: "",
      };
    },
    methods: {
      intentarDescubrir() {
        //Descubrir el Pokémon
        this.$emit("descubrirPokemon", { inputName: this.inputName, index: this.index });
      },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  .pokemon-card {
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    background: #fff;
    transition: transform 0.2s ease-in-out;
  }
  
  .pokemon-card:hover {
    transform: scale(1.05);
  }
  
  img {

    transition: filter 0.3s ease;
  }
  
  .filtered {
    filter: blur(5px) grayscale(100%);
  }
  
  input,
  button {
    width: 100px;
    padding: 5px;
    font-size: 14px;
    border-radius: 4px;
    border: 1px solid #ccc;
    text-align: center;
    outline: none;
  }
  
  input:focus,
  button:hover {
    border-color: #095a9a;
  }
  
  button {
    background: #ffcb03;
    color: #095a9a;
    font-weight: bold;
    cursor: pointer;
  }
  
  button:hover {
    background: #095a9a;
    color: #fff;
  }
  </style>
  