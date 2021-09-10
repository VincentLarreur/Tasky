<template lang="html">
  <div id="todo">
    <div id="nav">
      <button @click="filter=''" v-bind:class="{ current: filter === '' }">
        ALL
      </button>
      <button @click="filter='unfinished'" v-bind:class="{ current: filter === 'unfinished' }">
        ONGOING
      </button>
      <button @click="filter='done'" v-bind:class="{ current: filter === 'done' }">
        COMPLETED
      </button>
      <span class="underline">
        <span v-bind:class="{ done: filter === 'done', unfinished: filter === 'unfinished' }"/>
      </span>
    </div>

    <draggable
        class="list-group"
        tag="ul"
        v-model="filteredTaskList"
        v-bind="dragOptions"
        @start="drag = true"
        @end="drag = false"
      >
        <transition-group type="transition" :name="!drag ? 'flip-list' : null">
          <li
            class="list-group-item"
            v-for="element in filteredTaskList"
            :key="element.id"
          >
            <input type="checkbox" class="checkbox-round" v-model="element.state"/>
            <p class="title">{{ element.task }}</p>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16" @click="deleteTask(element.id)">
              <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
              <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
            </svg>
          </li>
        </transition-group>
      </draggable>

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
import draggable from 'vuedraggable'

const storageKey = 'taskList';

@Component({ components: { draggable } })
export default class Todos extends Vue {
  private id = 0;
  private task = '';
  private filter = '';
  // eslint-disable-next-line
  private taskList = [] as any;
  private filteredTaskList = [];
  private drag = false;
  private dragOptions = {
    animation: 200,
    group: "description",
    disabled: false,
    ghostClass: "ghost"
  };

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
    this.filterList();
  }

  @Watch('filter', { immediate: false, deep: true })
  onFilterChanged() {
    this.filterList();
  }

  addTask() {
    if(this.task != '') {
      this.taskList.push({
        id: this.id,
        task: this.task,
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
        this.filteredTaskList =  this.taskList.filter(task => task.state == true)
        return;
      }
      if(this.filter == 'unfinished') {
        this.filteredTaskList = this.taskList.filter(task => task.state == false)
        return;
      }
    }
    this.filteredTaskList = this.taskList;
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
  cursor: pointer;
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

.list-group {
  overflow: auto;
  list-style-type: none;
  padding: 0;
  margin: 5px;
}

.list-group-item {
  border-radius: 4px;
  background-color: rgb(60, 76, 162);
  color: white;
  margin: 5px;
  display: grid;
  grid-template-columns: 25px 1fr 25px;
  cursor: move;
}

.list-group-item .checkbox-round {
  width: 10px;
  height: 10px;
  margin: 13px 10px 10px 8px;
  border-radius: 50%;
  appearance: none;
  -webkit-appearance: none;
  outline: none;
  cursor: pointer;
  border-color: rgb(22, 28, 61);
  background-color: transparent;
}
.list-group-item .checkbox-round:checked {
  background-color: rgb(252, 148, 0);
}
.list-group-item .title {
  padding: 10px 0;
  margin: 0;
  font-size: 14px;
}

.list-group-item .bi-trash {
  cursor: pointer;
  margin: 10px 5px 0px;
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

.underline .unfinished {
  width: 30%;
  transform: translateX(75px);
}
.underline .done {
  width: 38%;
  transform: translateX(195px);
}
</style>