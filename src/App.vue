<template>
  <div class="todoContainer">
    <h1>To do list</h1>
    <div class="todoAction">
      <button @click="toggleFormModal">Ajouter une tache</button>
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
    <Modal :isOpen="isFormModalOpen" @close="isFormModalOpen = false">
      <template #header>
        <div class="formHeader">
          <h2>Ajout d'une tâche</h2>
        </div>
      </template>

      <template #body>
        <form @submit.prevent="submitForm" class="formBody">
          <div class="container">
            <div class="col">
              <input type="text" placeholder="Titre de la tâche" v-model="newTask.title">
            </div>
            <div class="col">
              <input type="number" placeholder="Durée en heure" v-model="newTask.duree">
            </div>
          </div>
          <textarea placeholder="Description de la tâche" v-model="newTask.description" />
          <VueDatePicker locale="fr" cancelText="annuler" selectText="confimer" placeholder="Date de début" v-model="newTask.startingDate"/>
        </form>
      </template>

      <template #footer>
        <div class="formFooter">
          <button @click="addTask">Confirmer</button>
          <button @click="toggleFormModal">Annuler</button>
        </div>
      </template>
    </Modal>
  </div>
</template>

<script setup>
  import {ref, computed} from "vue"
  import Modal from "./composant/Modal.vue";
  import VueDatePicker from '@vuepic/vue-datepicker';
  import '@vuepic/vue-datepicker/dist/main.css'

  const tasks = ref([])
  const newTask = ref({
    startingDate: new Date(), 
    title: "",
    description: "",
    duree: ""
  })

  const hidder = ref(false)

  const sortTask= () => {
    tasks.value.sort((a, b) => a.completed - b.completed);
  }

  const addTask = () =>{
    const format = (date = newTask.value.startingDate) => {
      const day = date.getDate()
      const month = date.getMonth() + 1
      const year = date.getFullYear()

      return `${day}/${month}/${year}`
    }
    tasks.value.push({
      title: newTask.value.title,
      description: newTask.value.description,
      completed: false,
      date: format(),
      duree: newTask.value.duree
    })
    newTask.value = {
      startingDate: new Date(), 
      title: "",
      description: "",
      duree: ""
    }
    sortTask()
  }

  const filteredTasks = computed(() => {
    return tasks.value.filter(task => !(hidder.value && task.completed))
  })

  const isFormModalOpen = ref(false)
  const toggleFormModal =()=>{
    isFormModalOpen.value = !isFormModalOpen.value
  }

</script>

<style lang="scss">
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

  .formBody{
    display: flex;
    flex-direction: column;
    align-items: center;
    .container{
      display: flex;
      flex-direction: row;
      margin-bottom: 3%;
      .col{
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 50%;
        input{
          width: 90%;
        }
      }
    }
    textarea{
      width: 95%;
      margin-bottom: 3%;
    }
    .dp__main, .dp__theme_light{
      width: 98%;
    }
  }
  
  .formFooter{
    margin-top: 10%;
    display: flex;
    justify-content: center;
    button{
      margin: 0 1vh;
    }
  }
</style>