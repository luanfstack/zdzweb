<template>
  <v-container fluid>
    <v-form v-if="produto" ref="form" v-model="valid" lazy-validation>
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

      <v-btn
        :disabled="!valid"
        color="success"
        class="mr-4"
        @click="edit(produto.id)"
      >
        Salvar
      </v-btn>
    </v-form>

    <div v-else>
      <p>Produto inexistente</p>
      <NuxtLink to="/criar">Voltar</NuxtLink>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    produto: undefined,
    categorias: [],
    nomeRules: [
      (v) => !!v || "Nome é obrigatório",
      (v) =>
        (v && v.length > 5) || "Nome precisa de ter pelo menos 5 caracteres",
    ],
  }),
  mounted() {
    this.$axios(
      `https://localhost:7244/api/produto/${this.$route.params.id}`
    ).then((response) => (this.produto = response.data));
    this.$axios("https://localhost:7244/api/categoria/").then(
      (response) => (this.categorias = response.data.map((c) => {
          return {
            text: c.nome,
            value: c.id,
          };
        }))
    );
  },
  methods: {
    async edit() {
      try {
        await this.$axios({
          method: "PUT",
          url: `https://localhost:7244/api/produto/${this.produto.id}`,
          data: this.produto,
        });
      } catch (err) {
        console.log(err);
      } finally {
        this.$router.push("/produtos/lista");
      }
    },
  },
};
</script>

<style></style>
