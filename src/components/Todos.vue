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

    <input 
      type="text" 
      id="task_input"
      placeholder="ADD TASK"
      v-model="task"
      @keyup.enter="addTask"
    />
  </div>
</template>


<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class Todos extends Vue {
  id = 0;
  task = '';
  description = '';
  filter = '';
  // eslint-disable-next-line
  taskList = [] as any;

  addTask() {
    if(this.task != '') {
      this.taskList.push({
        id: this.id,
        task: this.task,
        descrption: this.description,
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
  font-size: 18px;
  font-family: "Ubuntu";
  text-transform: uppercase;
  text-align: center;
  background: none;
  border: none;
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
  background-color: rgb(21, 30, 81);
  opacity: 0.65;
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

#task_input {
  border-width: 3px;
  border-color: rgb(252, 148, 0);
  border-style: solid;
  background-color: rgba(9, 13, 37, 0.4);
  min-width: 150px;
  height: 40px;
  margin: 5px auto;
  border-radius: 40px;
  text-align: center;
}
</style>