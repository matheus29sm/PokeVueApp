<template>
  <v-dialog v-model="dialog" max-width="650">
    <v-toolbar dark color="red">
      <v-toolbar-title class="text-center"> 
        Movimentos do Pokémon 
      </v-toolbar-title>
      <v-btn icon dark @click="closeDialog">
        <v-icon>mdi-close-circle</v-icon>
      </v-btn>
    </v-toolbar>
    <v-card>
      <v-card-text>
        <div>
          <span v-for="(movesItem, index) in moves" :key="index">
            {{ movesItem.move.name }}<span v-if="index < moves.length - 1">, </span>
          </span>
        </div>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="red" text @click="closeDialog">Fechar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>

export default {
  name: 'MovesModal',

  props: {
    moves: {
      type: Array,
      required: true
    },
  },

  data() {
    return {
      dialog: true,
    };
  },
  
  methods: {
    closeDialog() {
      this.dialog = false;
      this.$emit('close');
    }
  }
};
</script>
