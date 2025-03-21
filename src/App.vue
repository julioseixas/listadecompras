<script setup>
import { ref, onMounted } from "vue";
import AdicionarItem from "./components/AdicionarItem.vue";
import ListaItens from "./components/ListaItens.vue";
import CategoriaItens from "./components/CategoriaItens.vue";

const listaCompras = ref([]);
const categorias = ["Alimentos", "Limpeza", "Higiene", "Outros"];
const API_URL = "http://localhost:3000/itens";

// 📌 Carregar os itens da API ao iniciar (GET)
const carregarItens = async () => {
  try {
    console.log("Carregando itens...");
    const resposta = await fetch(API_URL);
    if (!resposta.ok) throw new Error(`Erro ao carregar itens: ${resposta.status}`);
    listaCompras.value = await resposta.json();
  } catch (erro) {
    console.error("Erro ao carregar itens:", erro);
  }
};

// 📌 Adicionar um novo item (POST)
const adicionarItem = async (novoItem) => {
  try {
    console.log("📤 Enviando item para a API...", novoItem);
    const resposta = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ ...novoItem, comprado: false }),
    });

    if (!resposta.ok) throw new Error(`Erro ao adicionar item: ${resposta.status}`);

    const itemAdicionado = await resposta.json();
    listaCompras.value.push(itemAdicionado);
  } catch (erro) {
    console.error("Erro ao adicionar item:", erro);
  }
};

// 📌 Marcar ou desmarcar um item como comprado (PUT)
const marcarComoComprado = async (id, comprado) => {
  try {
    console.log(`📌 Marcando item ${id} como ${comprado ? "não comprado" : "comprado"}`);
    const resposta = await fetch(`${API_URL}/${id}`, {
      method: "PATCH", // 
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ comprado: !comprado }), 
    });

    if (!resposta.ok) throw new Error(`Erro ao atualizar item: ${resposta.status}`);

    const itemAtualizado = await resposta.json();
    const index = listaCompras.value.findIndex((item) => item.id === id);
    listaCompras.value[index].comprado = itemAtualizado.comprado;
  } catch (erro) {
    console.error("Erro ao atualizar item:", erro);
  }
};

// 📌 Remover um item (DELETE)
const removerItem = async (id) => {
  try {
    console.log("Removendo item com ID:", id);
    await fetch(`${API_URL}/${id}`, { method: "DELETE" });
    listaCompras.value = listaCompras.value.filter((item) => item.id !== id);
  } catch (erro) {
    console.error("Erro ao remover item:", erro);
  }
};

// 📌 Chamar a função quando o app for carregado
onMounted(carregarItens);
</script>

<template>
  <div class="container">
    <h1>🛒 Lista de Compras</h1>
    <AdicionarItem @adicionar-item="adicionarItem" :categorias="categorias" />
    <CategoriaItens :itens="listaCompras" />
    <ListaItens :itens="listaCompras" @remover-item="removerItem" @marcar-comprado="marcarComoComprado" />
  </div>
</template>



<style>

body {
  background-color: #f0f8ff; 
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

/* Estilização principal */
.container {
  max-width: 600px;
  margin: auto;
  text-align: center;
  padding: 20px;
  background: white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

/* Título */
h1 {
  color: #007bff;
  margin-bottom: 20px;
}

/* Estilização dos botões */
button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 15px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

/* Lista de itens */
ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #e3f2fd;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
}

button.delete {
  background-color: #dc3545;
}

button.delete:hover {
  background-color: #c82333;
}
</style>