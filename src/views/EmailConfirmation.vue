<template>
  <div class="columns" :class="{ loading: isLoading }">
    <div class="column is-4 is-offset-4 has-text-centered">
      <h1 class="title">Confirmando usuário</h1>
      <h2 :class="{ 'has-text-danger': isError }">
        {{ message }}
      </h2>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";

export default {
  props: {
    token: String
  },
  data() {
    return {
      message: "",
      isError: false,
      isLoading: true
    };
  },
  async created() {
    try {
      await this.confirm(this.token);
      this.message = "Email confirmado com sucesso";
    } catch ({ response }) {
      this.message = response.data.message;
      this.isError = true;
    } finally {
      this.isLoading = false;
    }
  },
  methods: {
    ...mapActions("login", ["confirm"])
  }
};
</script>
