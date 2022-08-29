<template>
  <v-container fluid>
    <v-form v-if="fornecedor" ref="form" v-model="valid" lazy-validation>
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

      <v-btn
        :disabled="!valid"
        color="success"
        class="mr-4"
        @click="edit(fornecedor.id)"
      >
        Salvar
      </v-btn>
    </v-form>
    <div v-else>
      <p>Fornecedor inexistente</p>
      <NuxtLink to="/fornecedor/lista">Voltar</NuxtLink>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    fornecedor: undefined,
    produtos: [],
    nomeRules: [
      (v) => !!v || "Nome é obrigatório",
      (v) =>
        (v && v.length > 5) || "Nome precisa de ter pelo menos 5 caracteres",
    ],
  }),
  mounted() {
    this.$axios(
      `https://localhost:7244/api/fornecedor/${this.$route.params.id}`
    ).then((response) => (this.fornecedor = response.data));
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
    async edit() {
      try {
        await this.$axios({
          method: "PUT",
          url: `https://localhost:7244/api/fornecedor/${this.fornecedor.id}`,
          data: this.fornecedor,
        });
      } catch (err) {
        console.log(err);
      } finally {
        this.$router.push("/fornecedor/lista");
      }
    },
  },
};
</script>

<style></style>
