<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>KANBAN VUEFIRE</h1>
        <button class="btn btn-primary" data-toggle="modal" data-target="#myModal">Add New Task</button>
        <hr>
        <!-- backlog -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h3>Backlog</h3>
              </div>
            </div>
          </div>
        </div>
        
        <!-- todo -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h3>Todo</h3>
              </div>
            </div>
          </div>
        </div>
        
        <!-- in progress -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h3>On Going</h3>
              </div>
            </div>
          </div>
        </div>
        
        <!-- done -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h3>Done</h3>
              </div>
            </div>
          </div>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
// Initialize Firebase
const config = {
  apiKey: 'AIzaSyCZ63S4sNjwImgyh278xFvoSSWnlTNemiM',
  authDomain: 'kanban-vue-firebase.firebaseapp.com',
  databaseURL: 'https://kanban-vue-firebase.firebaseio.com',
  projectId: 'kanban-vue-firebase',
  storageBucket: 'kanban-vue-firebase.appspot.com',
  messagingSenderId: '23039019358'
}

let kanbanfireApp = Firebase.initializeApp(config)
let db = kanbanfireApp.database().ref('tasks')

export default {
  name: 'home',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      detailTaskModal: false,
      addTasks: {
        title: '',
        description: '',
        point: '',
        assign: '',
        status: 'backlog'
      },
      currentTask: null
    }
  },
  firebase: {
    backlogs: db
  },
  methods: {
    addTask () {
      db.push(this.addTasks)
      this.addTasks.title = ''
      this.addTasks.description = ''
      this.addTasks.point = ''
      this.addTasks.assign = ''
    },
    showDetail (backlog) {
      console.log(backlog['.key'])
      this.currentTask = backlog
    },
    removeTask (tasks) {
      console.log(tasks['.key'])
      db.child(tasks['.key']).remove()
    },
    backlog (tasks) {
      let data = db.child(tasks['.key'])
      data.update({
        'status': 'todo'
      })
    },
    doing (tasks) {
      let data = db.child(tasks['.key'])
      data.update({
        'status': 'doing'
      })
    },
    done (tasks) {
      let data = db.child(tasks['.key'])
      data.update({
        'status': 'done'
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
