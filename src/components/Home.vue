<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>KANBAN VUEFIRE BOARD</h1>
        <br>
        <button class="btn btn-primary" data-toggle="modal" data-target="#myModal">ADD NEW TASK</button>
        <hr>
        <!-- backlog -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="panel panel-danger">
                <div class="panel panel-heading"><h3><strong>BACKLOG</strong></h3></div>
                <div class="panel-body">
                  <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'backlog'">
                  <div class="caption">
                    <div>
                      <h4><strong>{{backlog.title}}</strong></h4>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-danger" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
                  </div>
                  
                  <!-- modal detail -->
                </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- todo -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="panel panel-warning">
                <div class="panel panel-heading"><h3><strong>TO DO</strong></h3></div>
                <div class="panel-body">
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'todo'">
                  <div class="caption">
                    <div>
                      <h4><strong>{{backlog.title}}</strong></h4>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-warning" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
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
              <div class="panel panel-success">
                <div class="panel panel-heading"><h3><strong>ON GOING</strong></h3></div>
                <div class="panel-body">
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'doing'">
                  <div class="caption">
                    <div>
                      <h4><strong>{{backlog.title}}</strong></h4>
                      <hr>
                      <p>Point: {{backlog.point}}</p>
                      <p>Assigned To: {{backlog.assign}}</p>
                      <button @click="showDetail(backlog)" class="btn btn-success" data-toggle="modal" data-target="#modalDetail">Detail</button>
                    </div>
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
              <div class="panel panel-info">
                <div class="panel panel-heading"><h3><strong>DONE</strong></h3></div>
                <div class="panel-body">
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'done'">
                  <div class="caption">
                    <div>
                      <h4><strong>{{backlog.title}}</strong></h4>
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
                <input type="text" v-model="addTasks.title" class="form-control" id="title" placeholder="Title Task">
              </div>
              <div class="form-group">
                <label for="Point">Point: </label>
                <input type="text" v-model="addTasks.point" class="form-control" id="point" placeholder="0-100">
              </div>
              <div class="form-group">
                <label for="Description">Description</label>
                <textarea v-model="addTasks.description" class="form-control" rows="5" id="description" placeholder="Task Description"></textarea>
              </div>
              <div class="form-group">
                <label for="Assign">Assigned To: </label>
                <input type="text" v-model="addTasks.assign" class="form-control" id="assign" placeholder="People in Charge">
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
            <h4 class="modal-title">Detail Task</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title">{{currentTask.title}} for {{currentTask.assign}}</label>
                <p>Task Description: {{currentTask.description}}</p>
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
        'status': 'backlog'
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
