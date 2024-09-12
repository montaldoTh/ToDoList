<template>
  <div class="todoContainer">
    <h1>To do list</h1>
    <div class="todoAction">
      <form action="" @submit.prevent="addTask">
        <input type="text" placeholder="Ajoute une tache" v-model="newTask">
        <button>Ajouter</button>
      </form>
      <span style="margin-left: 10px; margin-top: 3px;">Masquer les taches terminées</span>
      <input type="checkbox" v-model="hidder">
    </div>
    <p v-if="tasks.length === 0">Aucunes taches à faire ! Ajoute en</p>
    <table v-if="tasks.length >= 1">
      <thead>
        <tr>
          <th scope="col">Taches à faire</th>
          <th scope="col">Date</th>
          <th scope="col">Option</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in filteredTasks" :key="task.title" @change="sortTask" :v-bind:class="task.completed ? 'completed' : 'uncompleted'" class="task">
          <td><p :style="{color: task.completed ? 'grey' : '', textDecoration: task.completed ? 'line-through' : ''}">{{ task.title }}</p></td>
          <td><p :style="{color: task.completed ? 'grey' : '', textDecoration: task.completed ? 'line-through' : ''}">{{ task.date }}</p></td>
          <td><input type="checkbox" v-model="task.completed"></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
  import {ref, computed} from "vue"

  const today = new Date()
  const formattedDate = today.toLocaleDateString('fr-FR')

  const tasks = ref([])
  const newTask = ref("")
  const hidder = ref(false)

  const sortTask= () => {
    tasks.value.sort((a, b) => a.completed - b.completed);
  }

  const addTask = () =>{
    tasks.value.push({
      title: newTask.value,
      completed: false,
      date: formattedDate
    })
    newTask.value = ''
    sortTask()
  }

  const filteredTasks = computed(() => {
    return tasks.value.filter(task => !(hidder.value && task.completed))
  })
</script>

<style>
  .todoContainer{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .todoAction{
    display: flex;
    flex-direction: row;
    margin-bottom: 15px;
  }

  h1{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  p{
    font-size: 17px;
  }

  span{
    font-size: 12px;
  }

  table{
    width: 384px;
  }
</style>