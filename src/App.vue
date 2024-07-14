<script setup>
import { reactive } from "vue";

const estado = reactive({
  filtro: "todas",
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
      finalizada: false,
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
  
}
</script>

<template>
  <div class="container">
    <header class="p-5 mt-4 mb-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form @submit="cadastrarTarefa">
      <div class="row">
        <div class="col">
          <input
            type="text"
            placeholder="Digite aqui a descrição da tarefa"
            class="form-control"
          />
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select
            @change="(evento) => (estado.filtro = evento.target.value)"
            class="form-control"
          >
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Concluídas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li
        class="list-group-item"
        v-for="tarefa in getTarefasFiltradas()"
        :key="tarefa.titulo"
      >
        <input
          @change="(evento) => (tarefa.finalizada = evento.target.checked)"
          type="checkbox"
          :checked="tarefa.finalizada"
          :id="tarefa.titulo"
        />
        <label
          :class="{ done: tarefa.finalizada }"
          class="ms-3"
          :for="tarefa.titulo"
        >
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
