<template lang="html">
  <div id="todo">
    <div id="nav">
      <button @click="filter=''" v-bind:class="{ current: filter === '' }">
        ALL
      </button>
      <button @click="filter='done'" v-bind:class="{ current: filter === 'done' }">
        ONGOING
      </button>
      <button @click="filter='unfinished'" v-bind:class="{ current: filter === 'unfinished' }">
        COMPLETED
      </button>
      <span class="underline">
        <span v-bind:class="{ done: filter === 'done', unfinished: filter === 'unfinished' }"/>
      </span>
    </div>

    <ul id="task_list">
      <li v-for="item in filterList()" :key="item.id">
        <div id="item">
          <input type="checkbox" v-model="item.state" />
          <p class="title"><b>{{ item.task }}</b><p>
          <p class="description">{{item.description}}</p>
        </div>
      </li>
    </ul>

    <div id="input">
      <input 
        type="text" 
        id="task_input"
        placeholder="ADD TASK"
        v-model="task"
        @keyup.enter="addTask"
      />
    </div>
  </div>
</template>


<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator';

const storageKey = 'taskList';

@Component
export default class Todos extends Vue {
  id = 0;
  task = '';
  description = '';
  filter = '';
  // eslint-disable-next-line
  taskList = [] as any;

  mounted() {
    const taskList = JSON.parse(window.localStorage.getItem(storageKey) || "[]");
    if (taskList.length > 0) {
      this.taskList = taskList;
      this.id = taskList.length;
    }
  }

  @Watch('taskList', { immediate: false, deep: true })
  onTaskListChanged() {
    window.localStorage.setItem(storageKey, JSON.stringify(this.taskList));
  }

  addTask() {
    if(this.task != '') {
      this.taskList.push({
        id: this.id,
        task: this.task,
        description: this.description,
        edit: false,
        state: false
      });
      this.task = '';
      this.id++;
    }
  }

  filterList() {
    if(this.filter !== '') {
      if(this.filter == 'done') {
        return this.taskList.filter(task => task.state == true)
      }
      if(this.filter == 'unfinished') {
        return this.taskList.filter(task => task.state == false)
      }
    }
    return this.taskList;
  }

  deleteTask(id) {
    this.taskList = this.taskList.filter(function(el) { return el.id != id; });
  }
}
</script>

<style>
#nav {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

#nav button {
  font-size: 16px;
  font-family: "Ubuntu";
  text-transform: uppercase;
  text-align: center;
  background: none;
  border: none;
  color: #434e8d;
  transition: all .3s ease-in-out;
}

#nav .current {
  color: rgb(252, 148, 0);
}

#todo {
  width: 350px;
  height: 70%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 4px;
  background-color: #1b2557;
  box-shadow: 0px 5px 150px 0px rgba(0, 7, 47, 0.75);
  display: grid;
  grid-template-rows: 70px 1fr 70px;
}

#task_list {
  overflow: auto;
  list-style-type: none;
  padding: 0;
  margin: 5px;
}

#task_list li {
  border-radius: 4px;
  background-color: rgb(60, 76, 162);
  color: white;
  margin: 5px;
}
#task_list input, #task_list .title {
  display: inline-block;
}

#input {
  display: flex;
  background-color: #1a224e;
}
input {
  color: rgb(252, 148, 0);
  width: 135px;
  border-width: 3px;
  border-color: rgb(252, 148, 0);
  border-style: solid;
  background-color: #131a3e;
  height: 40px;
  margin: auto;
  border-radius: 40px;
  text-align: center;
  font-size: 15px;
  transition: all .3s ease-in-out;
}
input:focus {
  outline: none;
  color: rgb(252, 148, 0);
  width: 250px;
}
input::placeholder {
  color: rgb(252, 148, 0);
}

.underline {
  position: absolute;
  width: 90%;
  top: 55px;
  left: 5%;
  border: solid 1px #434e8d;
}
.underline span {
  position: absolute;
  width: 20%;
  border: solid 1px rgb(252, 148, 0);
  border-radius: 1px;
  transition: all .3s ease-in-out;
}

.underline .done {
  width: 30%;
  transform: translateX(75px);
}
.underline .unfinished {
  width: 38%;
  transform: translateX(195px);
}
</style>