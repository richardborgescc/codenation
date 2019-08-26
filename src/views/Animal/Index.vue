<template>
  <div>
    <Navbar> </Navbar>
    <div class="container">
      <Bar>
        <button slot="right" class="button is-primary" @click="showModal">
          Novo Animal
        </button>
      </Bar>
      <Table class="is-fullwidth" :columns="columns" :rows="rows"> </Table>
    </div>
    <Modal @close="isOpened = false" title="Novo Animal" :is-opened="isOpened">
      <Form />
    </Modal>
  </div>
</template>

<script>
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
      columns: ["Id", "Nome", "Patas", "Tipo", "Data Nasc"],
      rows: [
        ["1", "Belinha", "3", "Mamifero", "10/12/2011"],
        ["2", "Tob", "4", "Mamifero", "10/12/2011"],
        ["3", "Tobias", "3", "Ave", "10/12/2011"]
      ]
    };
  },
  methods: {
    showModal() {
      this.isOpened = true;
    }
  }
};
</script>
