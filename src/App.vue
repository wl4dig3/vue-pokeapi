<template>
<div>
  <input v-model="nombre" type="text" @keyup.enter="buscarPokemon" />
  <button @click="buscarPokemon">Pokémon</button>
  <div>
    <img :src="imagen" alt />
    <h6>{{nombrePokemon}}</h6>
    <p v-for="(movimiento, i) in movimientos" :key="i">{{movimiento.move.name}}</p>
  </div>
</div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      nombre: "",
      pokemon: {
        name: "",
        sprites: {
          front_default: "",
        },
        moves: [],
      },
    };
  },
  created() {
    this.buscarPokemon();
  },
  methods: {
    buscarPokemon() {
      fetch(this.url)
        .then(function (response) {
          return response.json();
        })
        .then((myJson) => {
          console.log(myJson);
          this.pokemon = myJson;
        });
    },
  },
  computed: {
    imagen() {
      return this.pokemon.sprites.front_default;
    },
    nombrePokemon() {
      return this.pokemon.name;
    },
    movimientos() {
      return this.pokemon.moves;
    },
    url() {
      return this.nombre == "" ?
        `${this.setUrl}pikachu` :
        `${this.setUrl}${this.nombre.toLowerCase()}`;
    },
    setUrl() {
      return "https://pokeapi.co/api/v2/pokemon/";
    },
  },
};
</script>