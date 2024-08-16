<template>
  <div>
    <a href="https://vuejs.org/" target="_blank">
      <img src="../assets/vue.svg" class="logo" alt="Vue logo" />
    </a>
    <p>
        Requisição HTTP com axios em Vue.js
    </p>
    <v-container>
      <v-card>
        <v-data-table 
          :items="data"
          :headers="headers">
            <template v-slot:item="{ item }">
                <tr>
                  <td>{{ item.name }}</td>
                  <td>{{ item.id }}</td>
                  <td>
                    <v-btn
                    icon="mdi-eye"
                    variant="outlined"
                    size="small"
                    color="primary"
                    @click="navigateDetails(item.id)"
                    ></v-btn>
                  </td>
                </tr>
            </template>
        </v-data-table>
      </v-card>
    </v-container>
  </div>
</template>

<script>

import api from '../services/api.js'

export default {
  name: 'Home',
  data() {
    return {
      data: [],
      headers: [
        { title: 'Nome', value: 'name', align: 'center' },
        { title: 'Número Pokédex', value: 'id', align: 'center' },
        { title: 'Detalhes', value: 'detalhes', sortable: false, align: 'center' }
      ]
    };
  },

  created() {
    this.fetchData();
  },

  methods: {
    
    fetchData() {
      api.get("/pokemon?limit=151") // 151 1º geração,  o máximo da API e 1302.
      .then(response => {
        const results = response.data.results;
        results.forEach((pokemon, index) => {
          pokemon.id = index + 1;
        });
        this.data = results;
      })
      .catch(error => {
        console.error('Erro ao buscar dados:', error);
      });
    },

    navigateDetails(pokemonId) {
      this.$router.push(`/detalhes/${pokemonId}`);
    }
  }
};
</script>

<style>
.logo {
  height: 4em;
  padding: 1em;
}
</style>