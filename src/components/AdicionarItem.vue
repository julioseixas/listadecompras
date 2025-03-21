<script setup>
import { ref } from "vue";

const emit = defineEmits(["adicionar-item"]);
const props = defineProps(["categorias"]);

const nome = ref("");
const categoria = ref("Alimentos");

const adicionar = () => {
  if (nome.value.trim()) {
    emit("adicionar-item", { nome: nome.value, categoria: categoria.value });
    nome.value = "";
  }
};
</script>

<template>
  <div class="adicionar">
    <input v-model="nome" type="text" placeholder="Digite o item..." />
    <select v-model="categoria">
      <option v-for="cat in categorias" :key="cat">{{ cat }}</option>
    </select>
    <button @click="adicionar">➕ Adicionar</button>
  </div>
</template>

<style>
.adicionar {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

input, select, button {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #007bff;
  border-radius: 5px;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
