<template>
  <div class="columns">
    <div class="column is-4 is-offset-4">
      <h1 class="title has-text-centered">Cadastrar novo usuário</h1>
      <div class="notification is-danger" v-show="errors.length > 0">
        <p v-for="({ message }, index) in errors" :key="index">
          {{ message }}
        </p>
      </div>
      <form @submit.prevent="submit(form)">
        <div class="field">
          <input
            class="input"
            :class="{ 'is-danger': $v.form.name.$invalid }"
            v-model="form.name"
            placeholder="Nome"
          />
          <span class="help is-danger" v-show="$v.form.name.$invalid">
            Nome é obrigatório
          </span>
        </div>
        <div class="field">
          <input
            class="input"
            :class="{ 'is-danger': $v.form.email.$invalid }"
            v-model="form.email"
            placeholder="Email"
          />
          <span class="help is-danger" v-show="$v.form.email.$invalid">
            Email inválido
          </span>
        </div>
        <div class="field">
          <input
            class="input"
            :class="{ 'is-danger': $v.form.password.$invalid }"
            v-model="form.password"
            placeholder="Senha"
            autocomplete="new-password"
            type="password"
          />
          <span class="help is-danger" v-show="$v.form.password.$invalid">
            Senha é obrigatório
          </span>
        </div>

        <button class="button" :disabled="$v.form.$invalid">
          Cadastrar
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { required, minLength, email } from "vuelidate/lib/validators";
import { mapActions } from "vuex";

export default {
  data() {
    return {
      errors: [],
      form: {
        name: "",
        email: "",
        password: ""
      }
    };
  },
  validations: {
    form: {
      name: {
        required
      },
      email: {
        required,
        email
      },
      password: {
        required,
        minLength: minLength(8)
      }
    }
  },
  methods: {
    ...mapActions("login", ["signup"]),
    async submit(form) {
      try {
        await this.signup(form);
      } catch ({ response }) {
        this.errors = response.data.errors;
      }
    }
  }
};
</script>
