<template>
  <div>
    <Navbar>
      <div class="navbar-end">
        <div class="navbar-item">{{ user.name }}</div>
      </div>
    </Navbar>
    <div class="container">
      <Bar>
        <button
          v-if="hasPermission('users:write')"
          slot="right"
          class="button is-primary"
          @click="showModal"
        >
          Novo Usuário
        </button>
      </Bar>
      <Table class="is-fullwidth" :columns="columns" :rows="rows">
        <tr slot-scope="row">
          <td>{{ row.item.id }}</td>
          <td>{{ row.item.name }}</td>
          <td>{{ row.item.email }}</td>
          <td>
            <div
              class="tags has-addons is-marginless"
              v-for="(scope, index) in row.item.scope"
              :key="index"
            >
              <span :id="`span_${index}`" class="tag">{{ scope.object }}</span>
              <span class="tag is-primary">{{ scope.action }}</span>
            </div>
          </td>
        </tr>
      </Table>
    </div>
    <Modal @close="isOpened = false" title="Novo Animal" :is-opened="isOpened">
      <Form />
    </Modal>
  </div>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex";
import { domain } from "env";

import Table from "@/components/Table";
import Navbar from "@/components/Navbar";
import Bar from "@/components/Bar";
import Modal from "@/components/Modal";

import Form from "./Form";

export default {
  name: "app",
  components: { Table, Navbar, Bar, Modal, Form },
  data() {
    return {
      email: "",
      isOpened: false,
      columns: ["Id", "Nome", "Email", "Permissão"],
      rows: []
    };
  },
  async created() {
    const { data } = await axios.get(`${domain}/users`);
    this.rows = data.map(row => {
      row.scope = row.scope.map(scope => {
        const [object, action] = scope.split(":");
        return {
          object,
          action
        };
      });
      return row;
    });
  },
  computed: {
    ...mapGetters("login", ["user"])
  },
  methods: {
    showModal() {
      this.isOpened = true;
    },
    hasPermission(scope) {
      return this.user.scope.includes(scope);
    }
  }
};
</script>
z
