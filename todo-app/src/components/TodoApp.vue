<template>
  <div class="container" style="max-width: 600px;">
    <h2 class="text-center mt-5">ToDo List</h2>

    <!-- input -->
    <div class="d-flex mt-5">
      <input v-model="task" @keyup.enter="submitTask" type="text" placeholder="Enter task" class="form-control w-100">
      <!-- tạo input box cho phép user nhập nội dung và lưu vào biến task thông qua directive "v-model" -->
      <button type="submit" @click="submitTask" class="btn btn-color rounded-0">SUBMIT</button>
    </div>
    <!-- alert -->
    <div class="alert alert-warning alert-dismissible fade " :class="{ 'show': condition }" role="alert">
        <strong>Task already exist in your to do list!</strong> Please submit new task.
        <button type="button" class="close" data-dismiss="alert" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
    </div>

    <!-- Task Table -->
    <table class="table table-bordered mt-5 table-none-margin">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <!-- lặp qua một mảng các tasks và render ra các phần tử tương ứng -->
          <td>
            <span :class="{'finished': task.status === 'Finished'}">
              {{task.name}}
            </span>
          </td>
          <td style="width: 120px;">
            <span @click="changeStatus(index)" class="pointer" 
              :class="{'text-danger': task.status === 'To do',
              'text-warning': task.status === 'In progress'
              }"
            >
              {{task.status}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div>
      <button class="btn btn-right btn-color" @click="undoTask" :disabled="undoDisabled">
        UNDO
        <span class="fa fa-undo"></span>
      </button>
      <button class="btn btn-left btn-color" @click="redoTask" :disabled="redoDisabled">
        REDO
        <span class="fa fa-redo"></span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){
    return {
      task: '',
      editedTask: null,
      statuses: ["To do", "In progress", "Finished"],
      condition: false,
      history: [],
      currentIndex: -1,

      tasks: [
        {
          name: 'Do Gleee task!',
          status: 'To do'
        },
        {
          name: 'Running',
          status: 'In progress'
        },       
      ]
    }
  },

  computed: {
    undoDisabled() {
      return this.currentIndex <= 0;
    },
    redoDisabled() {
      return this.currentIndex >= this.history.length - 1;
    },
  },

  methods: {
    existTask() {
      const existingTask = this.tasks.find(t => t.name.toLowerCase() === this.task.toLowerCase());
      if (existingTask) {
        return true;
      }
      return false;
    },

    submitTask(){
      if(this.task.length === 0) return;

      if(this.existTask()){
        this.condition = true;
        return;
      }else {
        this.condition = false;
      }

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: 'To do'
        })
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = '';
      return console.log(this.tasks);

    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    undoTask() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.tasks = this.history[this.currentIndex];
      }
    },

    redoTask() {
      if (this.currentIndex < this.history.length - 1) {
        this.currentIndex++;
        this.tasks = this.history[this.currentIndex];
      }
    },
  }
};
</script> 

  
<style scoped>
  .pointer {
    cursor: pointer;
  }

  .finished {
    text-decoration: line-through;
  }

  .alert {
    /* padding: 0; */
    margin: 0;
    /* border: 0; */
  }
  .table-none-margin {
    margin-top: 0 !important;
  }

  .btn-left {
    float: right;
  }

  .btn-right {
    float: left;
  }

  .btn-color {
    background-color: #ccc;
  }

  .btn-color:hover {
    background-color: #555;
    color: white;
  }
  
</style>
