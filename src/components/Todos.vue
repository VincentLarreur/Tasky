<template lang="html">

  <section class="Todos">
    <h1>Todo List</h1>
    <div>
      <input 
        type="text" 
        id="task_input"
        placeholder="Task"
        v-model="task">

      <button v-on:click="addTask">Add</button>
      <button v-on:click="deleteAll">Delete All</button>
      <button v-on:click="delFinished">Delete Done</button>
    </div>
    <div id="filters">
      <input type="radio" id="all" value="all" v-model="filter">
      <label for="all">All</label>
      <input type="radio" id="done" value="done" v-model="filter">
      <label for="done">Done</label>
      <input type="radio" id="unfinished" value="unfinished" v-model="filter">
      <label for="unfinished">Unfinished</label>
    </div>

    <ul id="task_list">
      <li v-for="item in filterList()" :key="item.id">
        <div id="item">
          <input type="checkbox" v-model="item.state">
          <div v-if="item.edit == false">
            <label v-on:dblclick="item.edit = true;">{{ item.task }}</label>
          </div>
          <input v-if="item.edit == true" v-model="item.task"
            v-on:keyup.enter="item.edit = false"
            v-on:blur="item.edit = false">
          <button v-on:click="deleteTask(item.id)">Delete</button>
        </div>
      </li>
    </ul>
     <footer v-if="this.taskList.length != 0">
        <span>Number of remaining tasks: {{ this.taskList.filter(task => task.state == false).length }}</span>
    </footer>
  </section>
</template>


<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class Todos extends Vue {
  id = 0;
  task = '';
  filter = '';
  // eslint-disable-next-line
  taskList = [] as any;

  addTask() {
    if(this.task != '') {
      this.taskList.push({
        id: this.id,
        task: this.task,
        edit: false,
        state: false
      });
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

  deleteAll() {
    this.taskList = [];
  }

  delFinished() {
    this.taskList = this.taskList.filter(task => task.state == false);
  }
}
</script>

<style>
  #task_list {
    padding: 0;
    list-style:outside none none;
  }

  li {
    padding: 5px;
    border-bottom: 1px solid #ccc;
  }

  input[type=text] {
    width: 130px;
    box-sizing: border-box;
    border: 2px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
    background-color: white;
    background-size: 22.5px;
    background-image: url('../../public/add_icon.png');
    background-position: 10px 10px; 
    background-repeat: no-repeat;
    padding: 12px 20px 12px 40px;
    transition: width 0.4s ease-in-out;
  }

  input[type=text]:focus {
    width: 60%;
  }

  button {
    font-size: 16px;
    padding: 6px 20px 6px 20px;
    border: 2px solid #ccc;
    margin-left: 15px;
  }

  #filters {
    margin-top: 15px;
  }

  #item {
    display: inline-flex;
    align-items: baseline;
  }
</style>
