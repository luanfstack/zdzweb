<template>
  <v-container fluid>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="fornecedor.nome"
        :min="5"
        :rules="nomeRules"
        label="Nome do Fornecedor"
        required
      ></v-text-field>

      <v-select
        v-model="fornecedor.produto"
        :items="produtos"
        label="Produto"
        required
      ></v-select>

      <v-btn :disabled="!valid" color="success" class="mr-4" @click="submit">
        Salvar
      </v-btn>
    </v-form>

    <v-data-table
      :headers="headers"
      :items="fornecedores"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    fornecedor: {
      id: 0,
      nome: "",
    },
    produtos: [],
    nomeRules: [
      (v) => !!v || "Nome é obrigatório",
      (v) =>
        (v && v.length > 5) || "Nome precisa de ter pelo menos 5 caracteres",
    ],
    headers: [
      {
        text: "Fornecedor",
        align: "start",
        sortable: false,
        value: "nome",
      },
    ],
    fornecedores: [],
  }),
  mounted() {
    this.$axios("https://localhost:7244/api/fornecedor").then(
      (response) => (this.fornecedores = response.data)
    );
    this.$axios("https://localhost:7244/api/produto").then(
      (response) =>
        (this.produtos = response.data.map((p) => {
          return {
            text: p.nome,
            value: p.id,
          };
        }))
    );
  },
  methods: {
    async submit() {
      try {
        await this.$axios({
          method: "POST",
          url: "https://localhost:7244/api/fornecedor",
          data: this.fornecedor,
        });
      } catch (err) {
        console.log(err);
      } finally {
        let response = await this.$axios(
          "https://localhost:7244/api/fornecedor"
        );
        this.fornecedores = response.data;
      }
    },
  },
};
</script>

<style></style>
