<template>
  <v-container fluid>
    <v-form v-if="categoria" ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="categoria.nome"
        :min="5"
        :rules="nomeRules"
        label="Nome da Categoria"
        required
      ></v-text-field>

      <v-btn :disabled="!valid" color="success" class="mr-4" @click="edit(categoria.id)">
        Salvar
      </v-btn>
    </v-form>
    <div v-else>
      <p>Categoria inexistente</p>
      <NuxtLink to="/criar">Voltar</NuxtLink>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    categoria: {
      id: 0,
      nome: "",
    },
    nomeRules: [
      (v) => !!v || "Nome é obrigatório",
      (v) =>
        (v && v.length > 5) || "Nome precisa de ter pelo menos 5 caracteres",
    ],
  }),
  mounted() {
    this.$axios(
      `https://localhost:7244/api/categoria/${this.$route.params.id}`
    ).then((response) => (this.categoria = response.data));
  },
  methods: {
    async edit() {
      try {
        await this.$axios({
          method: "PUT",
          url: `https://localhost:7244/api/categoria/${this.categoria.id}`,
          data: this.categoria,
        });
      } catch (err) {
        console.log(err);
      } finally {
        this.$router.push("/categorias/lista")
      }
    },
  },
};
</script>

<style></style>
