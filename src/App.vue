<script setup>
import { reactive } from "vue";
import Header from "./components/Header.vue";
import Form from "./components/Form.vue";
import List from "./components/List.vue";

const estado = reactive({
  filtro: "todas",
  novaTarefa: "",

  tarefas: [
    {
      titulo: "Estudar ES6",
      finalizada: false,
    },
    {
      titulo: "Estudar Sass",
      finalizada: false,
    },
    {
      titulo: "Ir para a academia",
      finalizada: true,
    },
  ],
});

const getTarefasPendentes = () => {
  return estado.tarefas.filter((tarefa) => !tarefa.finalizada);
};

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter((tarefa) => tarefa.finalizada);
};

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  if (filtro === "pendentes") {
    return getTarefasPendentes();
  } else if (filtro === "finalizadas") {
    return getTarefasFinalizadas();
  } else {
    return estado.tarefas;
  }
};

const cadastrarTarefa = () => {
  const novaTarefaobj = {
    titulo: estado.novaTarefa,
    finalizada: false,
  };
  estado.tarefas.push(novaTarefaobj);
  estado.novaTarefa = "";
};
</script>

<template>
  <div class="container">
    <Header :tarefasPendentes="getTarefasPendentes().length" />
    <Form
      :novaTarefa="estado.novaTarefa"
      :editarNovaTarefa="(event) => (estado.novaTarefa = event.target.value)"
      :cadastraTarefa="cadastrarTarefa"
      :trocarFiltro="(event) => (estado.filtro = event.target.value)"
    />
    <List :tarefas="getTarefasFiltradas()" />
  </div>
</template>

