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
              <v-card elevation="5">
                <strong>Status:</strong>
                <v-list>
                  <v-list-item v-for="(stat, index) in pokemon.stats" :key="index">
                    <v-list-item-title>
                      <strong>{{ stat.stat.name }}: </strong> {{ stat.base_stat }}
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-card>
            </div>
            <v-btn
              class="ma-2"
              color="blue"
              variant="outlined"
              @click="moves_dialog = true"
              append-icon="mdi-open-in-new"
            >
              Movimentos
            </v-btn>
            <moves-modal
              v-if="moves_dialog"
              :moves="pokemon.moves"
              @close="moves_dialog = false"
            ></moves-modal>
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
        color="primary"
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
import MovesModal from './MovesModal.vue';

export default {
  name: 'Details',

  props: {
    id: {
      type: String,
      required: true
    }
  },

  components: {
    MovesModal
  },

  data() {
    return {
      pokemon: {},
      nextDisabled: false,
      moves_dialog: false,
    };
  },

  created() {
    this.fetchPokemonDetails();
  },

  watch: {
    id() {
      this.fetchPokemonDetails();
      this.moves_dialog = false;
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

    goHome() {
      this.$router.push('/');
    },

    goNext() {
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
