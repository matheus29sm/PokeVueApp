<template>
  <v-container>
    <h1>Detalhes do Pokémon</h1>
    <v-row justify="center">
      <v-col cols="12" md="10">
        <v-card>
          <v-card-title>{{ pokemon.name }}</v-card-title>
          <v-card-subtitle>
            Número Pokédex: #{{ pokemon.id }}
          </v-card-subtitle>
          <v-card-text>
            <div>
              <strong>Habilidade(s): </strong>
              <span v-for="(abilities, index) in pokemon.abilities" :key="index">
                {{ abilities.ability.name }}<span v-if="index < pokemon.abilities.length - 1">, </span>
              </span>
            </div>
            <div>
              <strong>Altura:</strong> {{ parseFloat((pokemon.height * 0.1).toFixed(2)) }} metro(s)
            </div>
            <div>
              <strong>Peso:</strong> {{ parseFloat((pokemon.weight * 0.1).toFixed(2)) }} kg
            </div>
            <div>
              <strong>Tipo(s): </strong>
              <span v-for="(types, index) in pokemon.types" :key="index">
                {{ types.type.name }}<span v-if="index < pokemon.types.length - 1">, </span>
              </span>
            </div>
            <div>
              <strong>Status:</strong>
              <v-list dense>
                <v-list-item v-for="(stat, index) in pokemon.stats" :key="index">
                    <v-list-item-title> <strong>{{ stat.stat.name }}: </strong> {{ stat.base_stat }}</v-list-item-title>
                </v-list-item>
              </v-list>
            </div>
            <div>
              <strong>Movimento(s): </strong>
              <span v-for="(moves, index) in pokemon.moves" :key="index">
                {{ moves.move.name }}<span v-if="index < pokemon.moves.length - 1">, </span>
              </span>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <div>
      <v-btn
        class="ma-4"
        color="red"
        variant="outlined"
        @click="goBack"
      >
        Voltar
      </v-btn>
      <v-btn
        class="ma-4"
        color="red"
        variant="outlined"
        @click="goHome"
      >
        Home
      </v-btn>
      <v-btn
        class="ma-4"
        color="success"
        variant="outlined"
        @click="goNext"
        :disabled="nextDisabled"
      >
        Próximo
      </v-btn>
    </div>
    <v-alert v-if="nextDisabled" type="warning">
      Não há mais Pokémon para mostrar.
    </v-alert>
  </v-container>
</template>

<script>

import api from '../services/api.js'

export default {

  name: 'Details',
  props: ['id'],
  data() {
    return {
      pokemon: {},
      nextDisabled: false
    };
  },

  created() {
    this.fetchPokemonDetails();
  },

  watch: {
    id() {
      // console.log('ID do Pokémon atualizado:', id);
      this.fetchPokemonDetails();
    }
  },

  methods: {

    fetchPokemonDetails() {
      api.get(`/pokemon/${this.id}/`)
        .then(response => {
          this.pokemon = response.data;
        })
        .catch(error => {
          console.error('Erro ao buscar dados do Pokémon:', error);
        });
    },

    goBack() {
        this.$router.go(-1);
    },
    
    goHome(){
      this.$router.push('/');
    },

    goNext(){
      const id = Number(this.id);
      const next = id + 1;
      // Supondo que o limite máximo de IDs seja 151
      if (next <= 151) {
        this.$router.push(`/detalhes/${next}`);
      } else {
        this.nextDisabled = true;
      }
    }

  },
};
</script>
