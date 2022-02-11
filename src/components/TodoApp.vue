<template>
  <div class="container">
    <h2 class="text-center">My vue to-do app</h2>

    <!-- input -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
    </div>

    <!-- task table -->
    <table class="table table-bordered mt-5"><!-- mt: margin top -->
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
          <td>
            <span :class="{'finished' : task.status === 'finished'}">
              {{task.name}}
            </span>
          </td>
          <td style="width: 120px">
            <span class="pointer fw-bold text-white rounded-3 p-1" @click="changeStatus(index)"
              :class="{
                'bg-danger': task.status === 'to-do',
                'bg-warning': task.status === 'in-progress',
                'bg-success': task.status === 'finished',
                }"
            >
              {{ firstCharUpper(task.status) }}
            </span>
          </td>
          <td>
            <div class="text-center pointer" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center pointer" @click="deleteTask(index)">
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
      editedTask: null,
      availableStatuses: ['to-do','in-progress','finished'],
      tasks: [
        {
          name:'Steal banana',
          status:'to-do'
        },
        {
          name:'Eat shit',
          status:'in-progress'
        }
      ]
    }
  },

  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name:this.task,
          status:'to-do'
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";

    }, 

    deleteTask(index){
      this.tasks.splice(index,1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
