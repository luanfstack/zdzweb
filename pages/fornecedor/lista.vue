<template>
  <v-container fluid>
    <NuxtLink to="/fornecedor/criar">
      <v-btn>Adicionar Fornecedor</v-btn>
    </NuxtLink>

    <v-card v-if="fornecedores">
      <v-list-item
        v-for="fornecedor in fornecedores"
        v-bind:key="fornecedor.id"
      >
        <v-list-item-content>
          <v-list-item-title
            >{{ fornecedor.nome }}
            <NuxtLink :to="'/fornecedor/' + fornecedor.id">
              <v-btn>Editar</v-btn>
            </NuxtLink>
            <v-btn @click="remove(fornecedor.id)">Remover</v-btn>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-card>

    <v-card v-else>
      <v-card-title>Nenhum Fornecedor</v-card-title>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    fornecedores: [],
  }),
  mounted() {
    this.$axios("https://localhost:7244/api/fornecedor").then(
      (response) => (this.fornecedores = response.data)
    );
  },
  methods: {
    async remove(id) {
      console.log(id);
      try {
        await this.$axios({
          method: "DELETE",
          url: `https://localhost:7244/api/fornecedor/${id}`,
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
