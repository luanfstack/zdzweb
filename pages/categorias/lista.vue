<template>
  <v-container fluid>
    <NuxtLink to="/categorias/criar">
      <v-btn>Adicionar Categoria</v-btn>
    </NuxtLink>

    <v-card v-if="categorias">
      <v-list-item v-for="categoria in categorias" v-bind:key="categoria.id">
        <v-list-item-content>
          <v-list-item-title
            >{{ categoria.nome }}
            <NuxtLink :to="'/categorias/' + categoria.id">
              <v-btn>Editar</v-btn>
            </NuxtLink>
            <v-btn @click="remove(categoria.id)">Remover</v-btn>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-card>

    <v-card v-else>
      <v-card-title>Nenhuma Categoria</v-card-title>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    categorias: [],
  }),
  mounted() {
    this.$axios("https://localhost:7244/api/categoria").then(
      (response) => (this.categorias = response.data)
    );
  },
  methods: {
    async remove(id) {
      console.log(id);
      try {
        await this.$axios({
          method: "DELETE",
          url: `https://localhost:7244/api/categoria/${id}`,
        });
      } catch (err) {
        console.log(err);
      } finally {
        let response = await this.$axios(
          "https://localhost:7244/api/categoria"
        );
        this.categorias = response.data;
      }
    },
    async update() {
      this.$axios("https://localhost:7244/api/categoria").then(
        (response) => (this.categorias = response.data)
      );
    },
  },
};
</script>

<style></style>
