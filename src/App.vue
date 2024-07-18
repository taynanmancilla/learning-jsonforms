<template>
  <div>
    <h1>Dados do Usuário</h1>
    <!-- Botão q abre o Modal de criação de usuário -->
    <button @click="showModal = true">Create</button>
    <!-- Tabela -->
    <custom-table :schema="userSchema" :data="userData" />
    <!-- Modal de criação de usuário: -->
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <span class="close" @click="showModal = false">&times;</span>
        <h1>Novo Usuario</h1>
        <json-forms
          :data="newUserData"
          :renderers="renderers"
          :schema="userSchema"
          :uischema="uiSchema"
          @change="onNewUserChange"
        />
        <button class="myButton" @click="addNewUser">Submit</button>
      </div>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, provide } from 'vue';
import CustomTable from './components/CustomTable.vue';  // Importe o componente da tabela
import { JsonForms, JsonFormsChangeEvent } from "@jsonforms/vue"; // Certifique-se de ter importado o JSON Forms
import {
  defaultStyles,
  mergeStyles,
  vanillaRenderers,
} from "@jsonforms/vue-vanilla";

const myStyles = mergeStyles(defaultStyles, { 
  control: {
    label: "mylabel",
    input: 'myInput'
   } 
});

const renderers = [
  ...vanillaRenderers,
];

const userSchema = ref({
  type: "object",
  properties: {
    name: { type: "string", title: "Nome" },
    age: { type: "integer", title: "Idade" },
    email: { type: "string", format: "email", title: "Email" },
    phone: { type: "string", minLength: 10, title: "Telefone" }
  }
});

const uiSchema = ref({
  type: "HorizontalLayout",
  elements: [
    {
      type: "VerticalLayout",
      elements: [
        { type: "Control", scope: "#/properties/name" },
        { type: "Control", scope: "#/properties/age" },
      ]
    },
    {
      type: "VerticalLayout",
      elements: [
        { type: "Control", scope: "#/properties/email" },
        { type: "Control", scope: "#/properties/phone" }
      ]
    },
  ]
});

const userData = ref([
  { name: "Alice", age: 25, email: "alice@example.com", phone: "1234567890" },
  { name: "Bob", age: 30, email: "bob@example.com", phone: "1234567890" }
]);

const showModal = ref(false);
const newUserData = ref({});

const onNewUserChange = (event: JsonFormsChangeEvent) => {
  newUserData.value = event.data;
};

const addNewUser = () => {
  userData.value.push({ ...newUserData.value });
  newUserData.value = {};
  showModal.value = false;
};

// Provide de estilos do componentes do JSONForms
provide('styles', myStyles);
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 120px;
  margin-right: 120px;
}

.mylabel {
  color: darkslategrey;
}

.vertical-layout {
  margin-left: 10px;
  margin-right: 10px;
}

.myform {
  width: 640px;
  margin: 0 auto;
}

.text-area {
  min-height: 80px;
}

.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.myInput {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.myButton {
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
}
</style>