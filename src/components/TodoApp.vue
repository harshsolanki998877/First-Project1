<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Todo List</h2>

    <!-- Input -->
    <div class="d-flex mt-5 input-btn-parent">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
      />
      <button class="btn btn-warning submit_btn" @click="submitTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5 todo_list_table">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
             <img class="delete-ic-cl" src="@/assets/images/delete-ic.png" alt="">
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <img class="edit-ic-cl" src="@/assets/images/edit-stake-ic.svg" alt="">
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],

      /* Status could be: 'to-do' / 'in-progress' / 'finished' */
      tasks: [
        {
          name: "I have done todo-project.",
          status: "to-do",
        },
        {
          name: "I have working on bhartapp",
          status: "in-progress",
        },
        {
          name: "Sports-app",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
  },
};
</script>

<style scoped>
.submit_btn{
  background:linear-gradient(-180deg, #315195 0%, #14213d 100%);
  border:none;
  color:#fff;
  border-radius:0px;
}
.input-btn-parent{
  gap:5px;
}
.input-btn-parent input{
  box-shadow:none;
  border-radius:0px;
}
.todo_list_table td{
text-transform: capitalize;
font-size:13px;
color:#666;
font-weight:500;
}

.todo_list_table thead th{
  background:linear-gradient(-180deg, #315195 0%, #14213d 100%);
  color:#fff;
  font-size:13px;
}
.delete-ic-cl{
  filter: brightness(0) saturate(100%);
}
.edit-ic-cl{
  filter: brightness(0) saturate(100%);
  width:20px;
  height:20px;
}
</style>