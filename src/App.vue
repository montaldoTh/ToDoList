<template>
  <div class="todoContainer">
    <h1>To do list</h1>
    <div class="todoAction">
      <button @click="toggleFormModal" class="addBtn">Ajouter une tache</button>
    </div>
    <p v-if="tasks.length === 0">Aucunes taches à faire ! :0</p>
    <div class="tasksContainer" v-if="tasks.length >= 1">
      <div class="task" v-for="task in filteredTasks" :key="task.title" @change="sortTask" :v-bind:class="task.completed ? 'completed' : 'uncompleted'">
        <div class="taskContent">
          <h3>{{ task.title }}</h3>
          <div class="customHr hrTitle"></div>
          <p id="description">{{ task.description }}</p>
          <div class="customHr hrDesc"></div>
          <p id="duree">Durée estimée à <span>{{ task.duree }}</span> heures</p>
          <p id="date">Créer le <span>{{ task.date }}</span></p>
        </div>
        <div class="taskOption">
          <button class="endBtn" @click="task.completed = !task.completed, sortTask">Terminer</button>
          <button class="editBtn">Modifier</button>
          <button class="cancelBtn">Annuler</button>
        </div>
      </div>
    </div>
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
          <VueDatePicker locale="fr" cancelText="annuler" selectText="confimer" placeholder="Date de début" v-model="newTask.date"/>
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
    date: new Date(), 
    title: "",
    description: "",
    duree: ""
  })

  const hidder = ref(false)

  const sortTask= () => {
    tasks.value.sort((a, b) => a.completed - b.completed);
  }

  const addTask = () =>{
    console.log(newTask.value);
    const format = (date = newTask.value.date) => {
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
      duree: newTask.value.duree.toString()
    })
    newTask.value = {
      date: new Date(), 
      title: "",
      description: "",
      duree: ""
    }
    console.log(tasks.value);
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

<style lang="scss" scoped>
  .todoContainer{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 800px;
    min-width: 360px;
    max-height: 872px;
    max-width: 484px;
    border-radius: 5px;
    background-color: #586BA4;
  }

  .todoAction{
    display: flex;
    flex-direction: row;
    margin-bottom: 22px;
  }

  button{
    background-color: #fff;
    border: 1px solid #d5d9d9;
    border-radius: 8px;
    box-shadow: rgba(81, 83, 83, 0.5) 0 2px 5px 0;
    box-sizing: border-box;
    color: #0f1111;
    cursor: pointer;
    display: inline-block;
    font-size: 13px;
    line-height: 29px;
    padding: 0 10px 0 11px;
    text-align: center;
    text-decoration: none;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
    vertical-align: middle;
    width: 100px;
    font-family: 'Quicksand';
    font-weight: 500;
  }

  button:hover {
    background-color: #f7fafa;
  }

  button:focus {
    border-color: #008296;
    box-shadow: rgba(81, 83, 83, 0.5) 0 2px 5px 0;
    outline: 0;
  }

  .addBtn{
    line-height: 17px;
    background-color: #8dccff;
    border-color: #2179c2;
  }

  .addBtn:hover{
    background-color: #2179c2;
    color: #fff;
  }

  .endBtn{
    background-color: #b1fab4;
    border-color: #2acc2f;
  }

  .endBtn:hover{
    background-color: #2acc2f;
    color: #fff;
  }

  .editBtn{
    background-color: #faefb1;
    border-color: #ddc539;
  }
  
  .editBtn:hover{
    background-color: #ddc539;
    color: #fff;
  }

  .cancelBtn{
    background-color: #fd9696;
    border-color: #bb2626;
  }

  .cancelBtn:hover{
    background-color: #bb2626;
    color: #fff;
  }

  .endBtn, .editBtn, .cancelBtn{
    width: 75px;
  }

  h1{
    font-family: 'Archivo Black';
    margin: 22px 0;
  }

  p{
    font-size: 17px;
    font-family: 'Quicksand';
    font-weight: 500;
  }

  span{
    font-size: 12px;
  }

  table{
    width: 384px;
  }

  .customHr{
    width: 80%;
    height: 1px;
    margin: 5px 0;
    background-color: #929292;
  }

  .hrTitle{
    margin-left: 35px;
  }

  .hrDesc{
    width: 70%;
  }

  .tasksContainer{
    width: 93%;
    display: flex;
    flex-direction: column;
    align-items: center;
    .task{
      background-color: #e6e6e6;
      width: 100%;
      border-radius: 8px;
      display: flex;
      flex-direction: row;
      box-shadow: 3px 5px 8px rgba(0, 0, 0, 0.5);
      margin-bottom: 15px;
      .taskContent{
        display: flex;
        flex-direction: column;
        width: 80%;
        margin: 0 3%;
        h3{
          margin: 0;
          margin-top: 5px;
          font-family: 'Quicksand';
          font-weight: 600;
        }
        p#description{
          margin: 0;
          font-size: 13px;
          font-family: 'Reddit Sans Condensed';
          font-weight: 400;
        }
        p#duree{
          margin: 0;
          font-size: 12px;
          font-weight: 500;
        }
        p#date{
          margin: 0;
          margin-bottom: 5px;
          margin-left: 5px;
          font-size: 10px;
          font-weight: 300;
          span{
            font-style: italic;
          }
        }
      }
      .taskOption{
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        margin: 1.5% 2%;
      }
    }
  }

  .task.completed{
    background-color: #d5ffbc;
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