<script setup>
import { reactive, computed, ref} from "vue"

const novaTarefa = ref("")
const tarefas = ref([])


function adicionarTarefa(){
  if(novaTarefa.value.trim() === "") return

  tarefas.value.push({ text: novaTarefa.value, concluida: false
   })
   
  novaTarefa.value = ""
}

function removerTarefa(index){
  tarefas.value.splice(index, 1)
}
function alternarStatus(index){
  tarefas.value[index].concluida =
    !tarefas.value[index].concluida
}
const filtro = reactive({
  status: "todas"
})

const tarefasFiltradas = computed(() => {
  if (filtro.status === "concluidas") {
    return tarefas.value.filter(t => t.concluida)
  }

  if (filtro.status === "pendentes") {
    return tarefas.value.filter(t => !t.concluida)
  }

  return tarefas.value
})
</script>

<template>
  <div class="container">
    <h1>Lista de tarefas</h1>
  <div class="filtros">
      <label for="filtroStatus">Filtrar tarefas:</label>

      <select id="filtroStatus" v-model="filtro.status">
        <option value="todas">Todas</option>
        <option value="concluidas">Concluídas</option>
        <option value="pendentes">Pendentes</option>
      </select>
    </div>
    
    <input 
      v-model="novaTarefa"
      placeholder="Digite uma tarefa"
    />

    <button @click="adicionarTarefa">
      adicionar
    </button>

    <ul>
      <li v-for="(tarefa, index) in tarefasFiltradas" :key="index"> 
  <span
    :class="{ concluida: tarefa.concluida }" 
    @click="alternarStatus(index)"
  >
        {{ tarefa.text}}
</span>
        <button @click="removerTarefa(index)">
          remover
        </button>

      </li>
    </ul>
 
  </div>
</template>

<style>
.container{
  max-width: 420px;
  margin: 60px auto;
  padding: 25px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.08);
  font-family: system-ui, sans-serif;
}
h1{
  margin-bottom: 20px;
  color: #333;
}

input{
width: 70%;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ddd;
  outline: none;
}
input:focus{
  border-color: #6366f1;
}

button{
   padding: 10px 14px;
  border-radius: 8px;
  border: none;
  background: #6366f1;
  color: white;
  cursor: pointer;
  transition: 0.2s;
}
button:hover{
  background: #4f46e5;
}

ul{
  list-style: none;
  padding: 0;
  margin-top: 20px;
}
li{
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f9fafb;
  padding: 10px;
  margin-bottom: 8px;
  border-radius: 8px;
}
.concluida{
  text-decoration: line-through;
  color: gray;
}

</style>