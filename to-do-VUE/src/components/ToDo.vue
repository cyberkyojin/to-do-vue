<template>
  <form v-if="edit" @submit="editTask">
    <input type="text" placeholder="Edit task" class="input" v-model="taskE" required />
    <i class="add bi bi-check-lg"></i>
  </form>

  <form v-else @submit="addTask">
    <input type="text" placeholder="New task" class="input" v-model="task" required />
    <button class="add" type="submit">Add</button>
  </form>

  <h2>Tasks</h2>
  <div v-for="(task, i) in taskL" :key="i" id="task">

    <span
      :style="task.completed == true ? 'text-decoration: line-through' : ''"
      v-on:click="completed(task._id, task.completed)"
    >
      {{ task.task }}
    </span>
    <div id="buttons">
      <i class="bt bi bi-x-lg" v-on:click="rmTask(task._id)"></i>
      <i class="bt bi bi-pencil-square" v-on:click="editI(task.task, task._id)"></i>
    </div>
  </div>
</template>

<script>
// import { v4 as uuidv4 } from "uuid";
// import api from "@/services/api.js";
import axios from "axios";
import { watch } from '@vue/runtime-core';

export default {
  name: "ToDo",
  data() {
    return {
      taskL: [],
      edit: false,
      interm: null
    };
  },
  methods: {
    addTask(e) {
      e.preventDefault();

      axios({
        method: "post",
        url: "http://localhost:8069/create",
        data: {
          task: this.task,
        },
      });

      this.task = "";
    },
    rmTask(id) {
      axios({
        method: "delete",
        url: `http://localhost:8069/delete/${id}`,
      });
    },
    completed(id, trueOrFalse) {
      const tf = !trueOrFalse;
      axios({
        method: "patch",
        url: `http://localhost:8069/tf/${id}/${tf}`,
      });
    },
    editTask(e) {
      e.preventDefault()
      axios({
        method: "patch",
        url: `http://localhost:8069/update/${this.interm}`,
        data: {
          taskE: this.taskE,
        },
      });
      
      this.edit = !this.edit
      
    },
    editI(task, id) {
      this.edit = !this.edit;
      this.interm = id
      this.taskE = task;
    },
  },
  created() {
    axios.get("http://localhost:8069/").then((r) => {
      // r.data.tasks.forEach(t => this.taskL.push(t))
      this.taskL = r.data.tasks
    })
  },
  
  watch: {
    taskL: {
      handler(val, oldVal) {
        axios.get("http://localhost:8069/").then((r) => {
          this.taskL = r.data.tasks
        })
      },
      deep: true
    },
  }
  
};
</script>

<style scoped>
.add {
  background-color: #e858cb;
	height: 30px;
	border-radius: 5px;
  padding: 4px;
	width: 40px;
	font-size: 16px;
	color: #141414;
	font-weight: bold;
	cursor: pointer;
	border: none;
}

.bt {
  background-color: #444;
	border: none;
	font-size: 19px;
	color: #e858cb;
  margin-right: 5px;
}

.input {
  height: 30px;
	padding: 0 10px;
	border-radius: 5px;
	border: none;
	flex: 2;
	background-color: #444;
	color: #eee;
	font-size: 16px;
  margin-right: 5px;
}

h2 {
  border-bottom: 1px solid #e858cb;
  color: #e858cb;
}

#task {
  height: 40px;
  background-color: #444;
	margin: 8px 0;
  padding-left: 10px;
	display: flex;
	border-radius: 5px;
	color: #eee;
	align-items: center;
  justify-content: space-between;
}

i {
  cursor: pointer;
  margin-right: 3px;
}


span {
  cursor: pointer;
  margin-right: 60px;
}
</style>