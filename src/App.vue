<template>
  <div class="card">
    <md-card>
      <h1>To-do list</h1>
      <md-field>
        <md-input v-model="currentTodo" @keydown.enter="addTodo()" placeholder="Add an item"></md-input>
      </md-field>
      <ul class="todos">
        <li class="todo-item" v-for="todo in todos" :key="todo.id">
          <input
            class="completed"
            type="checkbox"
            @change="togglecompleted"
            v-model="todo.completed"
          />
          <span
            v-if="todo.editing === false"
            class="editing"
            :class="{editing: todo == editedTodo}"
            @dblclick="editTodo(todo)"
          >{{ todo.label }}</span>
          <md-field v-if="todo.editing" class="editing">
            <md-input
              type="text"
              v-model="todo.label"
              @keyup.enter="stopEdit(todo)"
              @keyup.esc="stopEdit(todo)"
              @focusout="stopEdit(todo)"
              placeholder="Add item here"
            />
          </md-field>
          <button v-if="todo.editing === false" @click="editTodo(todo)">Edit</button>
          <button v-if="todo.editing === true" @click="stopEdit(todo)">Save</button>
          <button v-if="todo.editing === false" @click="removeTodo(todo)">Delete</button>
          <button v-if="todo.editing === true" @click="editTodo(todo)">Cancel</button>
        </li>
      </ul>
    </md-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: "",
      editedTodo: ""
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false,
        editing: false
      });
      this.currentTodo = "";
    },
    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      const index = this.todos.indexOf(todo);
      this.todos[index].editing = !this.todos[index].editing;
    },
    stopEdit(todo) {
      todo.editing = false;
    }
  }
};
</script>

<style>
body {
  background-color: black;
}

.card {
  background-color: white;
  width: 50%;
  margin: 0 auto;
  padding: 50px;
  border-radius: 15px;
  margin-top: 20px;
}

.md-field {
  border: 2px solid black;
  border-radius: 8px;
  min-height: 0px !important;
  font-size: 20px !important;
  padding-top: 5px !important;
  padding-left: 5px !important;
}

@media (max-width: 768px) {
  .card {
    width: 90%;
  }
}
</style>
