<template>
  <div class="container">
    <h2 class="text-center margin-top">ToDoList</h2>

    <!-- input -->
    <div class="flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control">
      <!-- tạo input box cho phép user nhập nội dung và lưu vào biến task thông qua directive "v-model" -->
      <button @click="submitTask" class="btn btn-warn rounded">SUBMIT</button>
    </div>

    <!-- Task Table -->
    <table class="table table-bordered margin-top">
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
          <th>{{ task.name }}</th>
          <td>{{task.status}}</td>
          <td>
            <div class="text-center">
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

  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      this.tasks.push({
        name: this.task,
        status: 'To do'
      })

      this.task = '';
    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    }
  }
};
</script>


<style scoped>

</style>
