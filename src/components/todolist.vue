<template>
  <div class="container">
    <h2 class="text-center">my vue todo app</h2>

    <!-- input -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="nhập task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
    </div>
    <!-- task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody v-for="(task, index) in tasks " :key="index">
        <tr>
          <td><span :class="{'finished':task.status==='finished'}">{{ task.name }}</span></td>
          <td style="width:120px"><span @click="changeStatus(index)" class="pointer" :class="{'text-danger': task.status==='to-do',
          'text-warning':task.status==='in-progress',
          'text-suscess':task.status==='in-finished'
          }">{{ firstCharUpper(task.status) }}</span></td>
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
  </div>
</template>
<script>
export default {
  data() {
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'steal bnanas from the store',
          status: 'to-do'
        },
        {
          name: 'eat 1kg chocolate in 1 hours',
          status: 'in-progress'
        }
      ]
    }
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = '';
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str){
      return str.charAt(0).toUpperCase() +str.slice(1);
    }
  }
};
</script>
<style scoped>
.pointer {
  cussor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>