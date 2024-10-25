<script setup>
import { reactive } from 'vue';

// Estado inicial das tarefas e do filtro
const estado = reactive({
  filtro: 'todas',
  tarefaTemporaria: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    },
  ]
})

// Funções para filtrar tarefas
const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes()

    case 'finalizadas':
      return getTarefasFinalizadas()

    default:
      return estado.tarefas
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemporaria,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova)
  estado.tarefaTemporaria = ''
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>

    <!-- Formulário de cadastro de tarefas -->
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemporaria" @change="evento => estado.tarefaTemporaria = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastro</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>

    <!-- Lista de tarefas filtradas -->
    <ul class="list-group mt-4">
      <!-- Itera sobre as tarefas filtradas -->
      <li v-for="tarefa in getTarefasFiltradas()" class="list-group-item">
        <!-- Checkbox para marcar/desmarcar como finalizada -->
        <input @change="evento => tarefa.finalizada = evento.target.checked" v-model="tarefa.finalizada" type="checkbox" id="tarefa.titulo">
        
        <!-- Label com estilo baseado no estado 'finalizada' -->
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
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