<template>
  <v-container fluid>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="categoria.nome"
        :min="5"
        :rules="nomeRules"
        label="Nome da Categoria"
        required
      ></v-text-field>

      <v-btn :disabled="!valid" color="success" class="mr-4" @click="submit">
        Salvar
      </v-btn>
    </v-form>

    <v-data-table
      :headers="headers"
      :items="categorias"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    categoria: {
      nome: "",
    },
    nomeRules: [
      (v) => !!v || "Nome é obrigatório",
      (v) =>
        (v && v.length > 5) || "Nome precisa de ter pelo menos 5 caracteres",
    ],
    headers: [
      {
        text: "Categoria",
        align: "start",
        sortable: false,
        value: "nome",
      },
    ],
    categorias: [],
  }),
  mounted() {
    this.$axios("https://localhost:7244/api/categoria").then(
      (response) => (this.categorias = response.data)
    );
  },
  methods: {
    async submit() {
      try {
        await this.$axios({
          method: "POST",
          url: "https://localhost:7244/api/categoria",
          data: this.categoria,
        });
      } catch (err) {
        console.log(err);
      } finally {
        await this.$axios({
          method: "GET",
          url: "https://localhost:7244/api/categoria",
        });
        this.categorias = response.data;
      }
    },
  },
};
</script>

<style></style>
