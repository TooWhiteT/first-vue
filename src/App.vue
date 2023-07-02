<template>
  <div id="app">
    <h1>待办事项清单</h1>
    <!-- 事件订阅 -->
    <TODOForm @todo-added="addTODO" />
    <h2 id="list-summary">{{ listSummary }}</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in listItems" :key="item.id">
        <TODOItem :label="item.label" :done="item.done" :id="item.id" @checkbox-changed="updateDoneStatus(item.id)"
          @item-deleted="deleteToDo(item.id)" @item-edited="editToDo(item.id, $event)" />
      </li>
    </ul>
  </div>
</template>

<script>
import TODOItem from "./components/TODOItem.vue";
import TODOForm from "./components/TODOForm.vue";
import uniqueId from "lodash.uniqueid";

export default {
  name: 'App',
  // 可以在这里本地注册组件。
  components: {
    TODOItem,
    TODOForm
  },
  data() {
    return {
      listItems: [
        { id: uniqueId("todo-"), label: "吃", done: true },
        { id: uniqueId("todo-"), label: "喝", done: false },
        { id: uniqueId("todo-"), label: "拉", done: false },
        { id: uniqueId("todo-"), label: "撒", done: false }
      ]
    }
  },
  methods: {
    addTODO(toDoLabel) {
      this.listItems.push({ id: uniqueId("todo-"), label: toDoLabel, done: false })
    },
    updateDoneStatus(toDoId) {
      const toDoToUpdate = this.listItems.find((item) => item.id === toDoId)
      toDoToUpdate.done = !toDoToUpdate.done
    },
    deleteToDo(toDoId) {
      const itemIndex = this.listItems.findIndex((item) => item.id === toDoId);
      this.listItems.splice(itemIndex, 1);
    },
    editToDo(toDoId, newLabel) {
      const toDoToEdit = this.listItems.find((item) => item.id === toDoId);
      toDoToEdit.label = newLabel;
    }
  },
  computed: {
    listSummary() {
      const numberFinishedItems = this.listItems.filter((item) => item.done).length
      return `${numberFinishedItems} out of ${this.listItems.length} items completed`
    }
  }
}
</script>

<style>
/* 全局样式 */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}

.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}

.btn__filter {
  border-color: lightgrey;
}

.btn__danger:focus {
  outline-color: #c82333;
}

.btn__primary {
  color: #fff;
  background-color: #000;
}

.btn-group {
  display: flex;
  justify-content: space-between;
}

.btn-group>* {
  flex: 1 1 auto;
}

.btn-group>*+* {
  margin-left: 0.8rem;
}

.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}

[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}

[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}

@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}

.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}

[class*="stack"]>* {
  margin-top: 0;
  margin-bottom: 0;
}

.stack-small>*+* {
  margin-top: 1.25rem;
}

.stack-large>*+* {
  margin-top: 2.5rem;
}

@media screen and (min-width: 550px) {
  .stack-small>*+* {
    margin-top: 1.4rem;
  }

  .stack-large>*+* {
    margin-top: 2.8rem;
  }
}

/* 全局样式结束 */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}

@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}

#app>* {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}

#app>form {
  max-width: 100%;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}
</style>
