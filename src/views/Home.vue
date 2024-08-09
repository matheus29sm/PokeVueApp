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
                </tr>
            </template>
        </v-data-table>
      </v-card>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios';

export default {

  data() {
    return {
      data: [],
      headers: [
        { title: 'Nome', value: 'name', align: 'center' },
        { title: 'Número Pokédex', value: 'id', align: 'center' }
      ]
    };
  },

  created() {
    this.fetchData();
  },

  methods: {
    fetchData() {
      axios.get('https://pokeapi.co/api/v2/pokemon?limit=151') // Substitua pelo URL da API
        .then(response => {
            const results = response.data.results;
            results.forEach((pokemon, index) => {
              pokemon.id = index + 1;
            });
            this.data = results;
            // let dataTemp = response.data.results.map(pokemon => pokemon.name);
            // console.log(dataTemp);
        })
        .catch(error => {
          console.error('Erro ao buscar dados:', error);
        });
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