<template>
  <v-container fluid>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="produto.nome"
        :min="5"
        :rules="nomeRules"
        label="Nome"
        required
      ></v-text-field>

      <v-text-field
        v-model="produto.quantidade"
        label="Quantidade"
        type="number"
        required
      ></v-text-field>

      <v-select
        v-model="produto.categoria"
        :items="categorias"
        label="Categoria"
        required
      ></v-select>

      <v-btn :disabled="!valid" color="success" class="mr-4" @click="submit">
        Salvar
      </v-btn>

    </v-form>

    <v-data-table
      :headers="headers"
      :items="produtos"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    produto: {
      nome: "",
      quantidade: 0,
      categoria: "",
    },
    categorias: [],
    nomeRules: [
      (v) => !!v || "Nome é obrigatório",
      (v) =>
        (v && v.length > 5) || "Nome precisa de ter pelo menos 5 caracteres",
    ],
    headers: [
      {
        text: "Produto",
        align: "start",
        sortable: false,
        value: "nome",
      },
      { text: "Quantidade", value: "quantidade" },
    ],
    produtos: [],
  }),
  mounted() {
    this.$axios("https://localhost:7244/api/categoria").then(
      (response) =>
        (this.categorias = response.data.map((c) => {
          return {
            text: c.nome,
            value: c.id,
          };
        }))
    );
    this.$axios("https://localhost:7244/api/produto").then(
      (response) => (this.produtos = response.data)
    );
  },
  methods: {
    async submit() {
      try {
        await this.$axios({
          method: "POST",
          url: "https://localhost:7244/api/produto",
          data: this.produto,
        });
        let response = await this.$axios({
          method: "GET",
          url: "https://localhost:7244/api/produto",
        });
        this.produtos = response.data;
      } catch(err) {
        console.log(err)
      }
    },
  },
};
</script>

<style></style>
