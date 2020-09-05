<template>
  <div class="container">
    <img class="img-fluid" width="900" src="../src/assets/pokemon_logo.png" alt />

    <h3 class="text-center my-5">Ingresa un numero y presiona 'enter' para buscar un Pokemon</h3>
    <input v-model="nombre" type="text" @keyup.enter="buscarPokemon" />
    <button class="btn btn-primary m-1 p-1" @click="buscarPokemon">Pokémon</button>
    <div class="row d-flex">
      <div class="principal">
        <img :src="imagen" alt />
        <h6>{{nombrePokemon}}</h6>
        <p v-for="(movimiento, i) in movimientos" :key="i">{{movimiento.move.name}}</p>
      </div>
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
      return this.pokemon.moves.slice(0, 5);
    },
    url() {
      return this.nombre == ""
        ? `${this.setUrl}pikachu`
        : `${this.setUrl}${this.nombre.toLowerCase()}`;
    },
    setUrl() {
      return "https://pokeapi.co/api/v2/pokemon/";
    },
  },
};
</script>

<style lang="scss" scoped>
.principal {
  background-image: url(../src/assets/poke.jpg);
  background-size: contain;
  background-repeat: no-repeat;
}
</style>