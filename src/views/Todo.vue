<template>
  <div class="home">
    <v-text-field
            v-model="newTaskTitle"
            @click:append="addTask"
            @keyup.enter="addTask"
            class="pa-3"
            outlined
            label="Adicionar Tarefa"
            append-icon="mdi-playlist-plus"
            hide-details
            clearable
            notnull
          ></v-text-field>
    <v-list class="pt-0"
      flat
    >
      <div
      v-for="task in tasks"
        :key="task.id"
      >
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'blue lighten-5' : task.done }"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
              :class="{ 'text-decoration-line-through' : task.done }"
              >
              {{ task.title }} 
              </v-list-item-title>
            </v-list-item-content>
          <v-list-item-action>
            <v-btn 
            @click.stop="deleteTask(task.id)"
            icon
            >
             <v-icon color="primary lighten-1">mdi-delete</v-icon>
            </v-btn>
        </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>

     </v-list>
  <v-alert
      dense
      outlined
      type="error"
      v-if="newTaskTitleAlert"
    >
      Por gentileza, preencha a tarefa.
    </v-alert>
  
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data() {
    return {
      newTaskTitle: '',
      newTaskTitleAlert: false,
      tasks: [
        // {
        // id: 1,
        // title: 'Sou uma tarefa',
        // done: false
        // },
      ]
    }
  },

  mounted() {
    if (localStorage.getItem('tasks')) {
      try {
        this.tasks = JSON.parse(localStorage.getItem('tasks'));
      } catch(e) {
        localStorage.removeItem('tasks');
      }
    }
  },

  
  methods: {
    doneTask(id) {
      let task = this.tasks.filter(task => task.id === id) [0]
      task.done = !task.done
      this.saveNotes()
    },

    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
      this.saveNotes()
    },

    addTask() {

      if(!this.newTaskTitle) {
      this.newTaskTitleAlert = true
      }

      else {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false
      }
      this.tasks.push(newTask)
      this.newTaskTitle = ''
      this.newTaskTitleAlert = false
      this.saveNotes()
      }
    },

    saveNotes() {
      const parsed = JSON.stringify(this.tasks)
      localStorage.setItem('tasks', parsed)
    }
    
  }
}
</script>
