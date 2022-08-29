<template>
  <v-container fluid>
    <NuxtLink to="/produtos/criar">
      <v-btn>Adicionar Produto</v-btn>
    </NuxtLink>

    <v-list-item v-for="produto in produtos" v-bind:key="produto.id">
      <v-list-item-content>
        <v-list-item-title
          >{{ produto.nome }}
          <NuxtLink :to="'/produtos/' + produto.id">
            <v-btn>Editar</v-btn>
          </NuxtLink>
          <v-btn @click="remove(produto.id)">Remover</v-btn></v-list-item-title
        >
      </v-list-item-content>
    </v-list-item>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    produtos: [],
  }),
  mounted() {
    this.$axios("https://localhost:7244/api/produto").then(
      (response) => (this.produtos = response.data)
    );
  },
  methods: {
    async remove(id) {
      try {
        await this.$axios({
          method: "DELETE",
          url: `https://localhost:7244/api/produto/${id}`,
        });
      } catch (err) {
        console.log(err);
      } finally {
        let response = await this.$axios(
          "https://localhost:7244/api/produto"
        );
        this.produtos = response.data;
      }
    },
  },
};
</script>

<style></style>
