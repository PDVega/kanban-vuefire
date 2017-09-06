<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>KANBAN VUEFIRE</h1>
        <br>
        <button class="btn btn-primary" data-toggle="modal" data-target="#myModal">Add New Task</button>
        <hr>
        <!-- backlog -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h3>Backlog</h3>
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'backlog'">
                  <div class="caption">
                    <div>
                      <h5>{{backlog.title}}</h5>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
                  </div>
                  
                  <!-- modal detail -->
                </div>
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
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'todo'">
                  <div class="caption">
                    <div>
                      <h5>{{backlog.title}}</h5>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- on going -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h3>On Going</h3>
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'doing'">
                  <div class="caption">
                    <div>
                      <h5>{{backlog.title}}</h5>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
                  </div>
                </div>
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
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'done'">
                  <div class="caption">
                    <div>
                      <h5>{{backlog.title}}</h5>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        
      </div>
    </div>
    
    <!-- modal add -->
    <div class="modal fade" id="myModal" role="dialog">
      <div class="modal-dialog">
        <!-- modal content -->
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Kanban</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title">Title: </label>
                <input type="text" v-model="addTasks.title" class="form-control" id="title">
              </div>
              <div class="form-group">
                <label for="Point">Point: </label>
                <input type="text" v-model="addTasks.point" class="form-control" id="point">
              </div>
              <div class="form-group">
                <label for="Description">Description</label>
                <textarea v-model="addTasks.description" class="form-control" rows="5" id="description"></textarea>
              </div>
              <div class="form-group">
                <label for="Assign">Assigned To: </label>
                <input type="text" v-model="addTasks.assign" class="form-control" id="assign">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button class="btn btn-default" data-dismiss="modal">Close</button>
            <button @click="addTask" class="btn btn-default" data-dismiss="modal">Add Task</button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- modal show detail -->
    <div class="modal fade" v-if="currentTask !== null" id="modalDetail" role="dialog">
      <div class="modal-dialog">
        <!-- modal content -->
        <div class="modal-content">
          <div class="modal-header">
            <button class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Detail Task: {{currentTask.title}} for {{currentTask.assign}}</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title">Task Description: {{currentTask.description}}</label>
                <p>Point: {{currentTask.point}}</p>
                <p>Status: {{currentTask.status}}</p>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button v-if="currentTask.status == 'todo'" @click="backlog(currentTask)" class="btn btn-default danger" data-dismiss="modal">BACKLOG</button>
            
            <button @click="removeTask(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DELETE</button>
            
            <button v-if="currentTask.status == 'doing' || currentTask.status == 'backlog'" @click="todo(currentTask)" type="button" class="btn btn-default" data-dismiss="modal">TODO</button>
            
            <button v-if="currentTask.status == 'todo'  || currentTask.status == 'done'" @click="doing(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DOING</button>
            
            <button v-if="currentTask.status == 'doing'" @click="done(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DONE</button>
            
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
    todo (tasks) {
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
</style>
