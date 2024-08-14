<template>
  <div>
      <h1>Detalhes do Pokémon</h1>
      <h3> Número:  {{this.id}} </h3>
      <div>
        <v-btn
          color="red"
          variant="outlined"
          @click="goBack"
        >
          Voltar
        </v-btn>
        <v-btn
          color="red"
          variant="outlined"
          @click="goHome"
        >
          Home
        </v-btn>
        <v-btn
          color="success"
          variant="outlined"
          @click="goNext"
        >
          Próximo
        </v-btn>
      </div>
  </div>
</template>

<script>

export default {
  name: 'Detalhes',
  props: ['id'],

  methods: {
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
        console.log('Não há mais Pokémon para mostrar.');
        // Mudar depois, transforma em um alerta ou ocultar botão 
      }
    }
  },

};

</script>
