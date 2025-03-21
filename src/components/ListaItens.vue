<script setup>
const props = defineProps(["itens"]);
const emit = defineEmits(["remover-item", "marcar-comprado"]);
</script>

<template>
  <div v-if="itens.length > 0">
    <h3>📃 Itens Adicionados:</h3>
    <ul>
      <li v-for="item in itens" :key="item.id" :class="{ comprado: item.comprado }">
        <input type="checkbox" :checked="item.comprado" @change="emit('marcar-comprado', item.id, item.comprado)" />
        
        <span>{{ item.nome }} ({{ item.categoria }})</span>
        
        <button class="delete" @click="emit('remover-item', item.id)">❌</button>
      </li>
    </ul>
  </div>
  <p v-else>🛍️ Nenhum item na lista.</p>
</template>

<style>
h3 {
  color: #007bff;
}

/* Estilização da lista */
ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #e3f2fd;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
  cursor: pointer;
}

/* 🔹 Checkbox */
input[type="checkbox"] {
  margin-right: 10px;
  transform: scale(1.2);
  cursor: pointer;
}

/* 🔹 Estilo para itens comprados (riscado) */
.comprado span {
  text-decoration: line-through;
  color: gray;
  opacity: 0.7;
}
</style>
