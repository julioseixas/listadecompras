<script setup>
import { computed } from "vue";

const props = defineProps(["itens"]);

const categoriasAgrupadas = computed(() => {
  const grupos = {};
  props.itens.forEach((item) => {
    if (!grupos[item.categoria]) {
      grupos[item.categoria] = [];
    }
    grupos[item.categoria].push(item.nome);
  });
  return grupos;
});
</script>

<template>
  <div v-if="Object.keys(categoriasAgrupadas).length > 0">
    <h3>📂 Itens por Categoria:</h3>
    <div v-for="(itens, categoria) in categoriasAgrupadas" :key="categoria">
      <strong>{{ categoria }}</strong>
      <ul>
        <li v-for="(item, index) in itens" :key="index">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<style>
h3 {
  margin-top: 15px;
}
strong {
  display: block;
  margin-top: 10px;
}
</style>
