<template>
  <div class="caja">
    <input class="input" type="text" placeholder="Agrega una nueva tarea" v-model="TaskInput" @keyup.enter="SaveTask"/>
    <select v-model="order" @change="Sort">
      <option value="A">Menor a mayor</option>
      <option value="D">Mayor a menor</option>
    </select>
    <ToDoItems @item="DeleteTask" v-bind="task"  v-for="(task, index) in TaskDinamico" :key="index"/>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import  ToDoItems from './ToDoItems.vue';
import Task  from "@/data/Task";
 const TaskInput = ref('');
 const TaskDinamico = ref(Task);
const order = ref('A')
const Sort =()=>{
  if (order.value == "A") {
    TaskDinamico.value.sort((i, f) => i.id - f.id);
  } else {
    TaskDinamico.value.sort((i, f) => f.id - i.id);
  }
}
const SaveTask = () =>{
  const nextId = TaskDinamico.value.length + 1;
  if(TaskInput.value.length == 0){
    return alert('La tarea no puede estar vacia')
  }
  const FindTask =   TaskDinamico.value.find(i =>i.name.toLowerCase() == TaskInput.value.toLowerCase());
  if(FindTask != null){
   return alert('Ya existe esta tarea, asigne otra')
  }
  TaskDinamico.value.push(
    {
      id:nextId,
      name: TaskInput.value
    }
  )
  TaskInput.value= ''
  Sort()
}
const DeleteTask = (id:Number) =>{
  setTimeout(() => {
    if(confirm('¿Estas seguro de eliminar este item?')){
      const FindTask =   TaskDinamico.value.findIndex(i =>i.id == id);
      console.log(FindTask)
      if (FindTask !== -1) {
        TaskDinamico.value.splice(FindTask,1)
        console.log(TaskDinamico)
      }
    }
  }, 100);

  Sort()
}
</script>