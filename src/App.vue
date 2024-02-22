<script setup>
import { reactive } from 'vue';


const estado = reactive ({
  filtro:'todas',
  tarefaTemp:'',
  tarefas:[
    {
      titulo:'estudar es6',
      finalizada:false,
    },
    {
      titulo:'estudar sass',
      finalizada:false,
    },
    {
      titulo:'estudar gulp',
      finalizada:true,
    }
  ]
})

const getTarefaspendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}
const getTarefasfinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const tarefasFiltradas = () => {
  const {filtro} = estado;

  switch(filtro) {
    case 'Pendentes':
      return getTarefaspendentes();
    case 'Finalizadas':
      return getTarefasfinalizadas();
    default:
      return estado.tarefas    
  }
}

const cadastrarTarefas = () =>{
  const tarefaNova = {
    titulo:estado.tarefaTemp,
    finalizada:false
  }
  estado.tarefas.push(tarefaNova)
  estado.tarefaTemp = ''
}


</script>

<template>
 
<div class="container">
  <header class="p-5 mb-3 mt-3 bg-secondary rounded-3">
    <h1 class="text-white">Minhas tarefas</h1>
    <p class="text-white"><b>Você possui  {{ getTarefaspendentes().length }} tarefas pendentes</b></p>
  </header>
</div>
<div class="container">
  <form @submit.prevent="cadastrarTarefas">
  <div class="row">
    <div class="col">
      <input :value="estado.tarefaTemp" @change="evento=> estado.tarefaTemp = evento.target.value" required class="form-control" type="text" placeholder="Digite sua tarefa ">
    </div>
    <div class="col-md-2">
      <button class="btn btn-success ms-3" type="submit">Cadastrar tarefas</button>
    </div>
    <div class="col-md-2">
      <select @change="evento => estado.filtro = evento.target.value" class="form-control cursor">
        <option value="Todas">Todas tarefas</option>
        <option value="Pendentes">Pendentes</option>
        <option value="Finalizadas">Finalizadas</option>
      </select>
    </div>
  </div>
</form>
<ul class="list-group mt-4">
  <li v-for="tarefa in tarefasFiltradas()" class="list-group-item">
    <input @change="evento=> tarefa.finalizada = evento.target.checked"  :checked="tarefa.finalizada" id="tarefa.titulo" type="checkbox">
    <label :class="{done: tarefa.finalizada}" class="ms-3" :for="tarefa.titulo">
      {{ tarefa.titulo }}
    </label>

  </li>
</ul>
</div>
</template>

<style scoped>
.done{
  text-decoration: line-through;
}



</style>
