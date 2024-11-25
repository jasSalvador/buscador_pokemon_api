<template>
  <div>
    <div class="bg-danger pb-5">
      <h1 class="text-center mx-auto pt-5 mb-3 text-warning fw-bold">Buscador de Pokémon</h1>

      <div class="container d-flex flex-wrap justify-content-between align-items-center mx-auto">
        <div class="col-lg-5 col-md-7 col-sm-10 border border-secondary mt-5 bg-light">
          <div class="bg-secondary bg-opacity-50">
            <p class="ps-3 py-1">Buscador</p>
          </div>
          <div class="d-flex justify-content-center align-items-center mx-auto gap-3 p-3">
            <input type="text" v-model="searchPokemon" class="form-control" placeholder="Ingresa ID o Nombre">
            <button @click="searchPokemonName" class="btn btn-success">Buscar</button>
          </div>
        </div>

        <div class="col-lg-5 col-md-8 col-sm-10 border border-secondary mt-5 bg-light">
          <div class="bg-secondary bg-opacity-50">
            <p class="ps-3 py-1">Pokémon</p>
          </div>
          <div class="px-3">           
            <p class=""><span class="fw-bold">ID:</span> {{pokemon.id}}</p>
            <p class=""><span class="fw-bold">Nombre:</span> {{pokemon.name}}</p>
          </div>
        </div>
      </div>
    </div>

    <hr class="my-0 linea">
    
    <div class="text-center mx-auto mb-5 col-2">
      <img :src="pokemon.sprite" alt="Pokemon" v-if="pokemon.sprite" class="w-100 d-block">
    </div>

  </div>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  name: 'App',
  data() {
    return {
      searchPokemon: '',
      pokemon: {
        id: '',
        name: '',
        sprite: ''
      }
    }
  },
methods: {
    async searchPokemonName() {
      if (!this.searchPokemon) {
        alert('Debes ingresar un ID o Nombre');
        return;
      }

      try {
        const response = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${this.searchPokemon.toLowerCase()}`
        );

        const data = await response.json();

        //actualizar la info
        this.pokemon = {
          id: data.id,
          name: data.name,
          sprite: data.sprites.front_default
        };
      } catch (error) {
        console.error(error);
        alert('No se encontró el Pokémon');
        //limpiar
        this.pokemon = { id: '', name: '', sprite: '' };
      }
    }
  },
  created() {
    //pokémon por defecto
    this.searchPokemon = 'pikachu';
    this.searchPokemonName();
  }
});
</script>

<style>
  .linea {
    border: 8px solid #07000a;
    opacity: 8;
  }
</style>
