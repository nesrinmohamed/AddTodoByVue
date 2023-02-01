<template>
  <div class="todo-app">
    <input
      type="text"
      placeholder="what needs to be done"
      class="todo-input"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed" />
        <div
          class="todo-item-label"
          v-if="!todo.isEdit"
          @dblclick="editTodo(todo)"
          :class="{ completed: todo.completed }"
        >
          {{ todo.title }}
        </div>
        <input
          v-else
          type="text"
          v-model="todo.title"
          class="todo-item-edit"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
        />
      </div>
      <div class="remove-item" @click="removeItem(index)">&times;</div>
    </div>

    <div class="extra-container">
      <div>
        <label>
          <input
            type="checkbox"
            :checked="!anyRemaning"
            @change="checkAllTodos"
          />
          Check Labels
        </label>
      </div>
      <div>{{remaining}} items  left</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      idForTodo: 1,
      beforeEditCache: "",
  
      todos: [
        {
          id: 1,
          title: "finish Vue ScreenShot",
          completed: false,
          isEdit: false,
        },
        {
          id: 2,
          title: "Take over world",
          completed: false,
          isEdit: false,
        },
      ],
    };
  },
  directives: {
    focus: {
      intersted: function (el) {
        el.focus();
      },
    },
  },
  computed:{
    remaining(){
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaning(){
      return this.remaining !== 0
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      });
      (this.newTodo = ""), this.idForTodo++;
    },
    removeItem(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      todo.isEdit = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() === "") {
        todo.title = this.beforeEditCache;
      }
      todo.isEdit = false;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.isEdit = false;
    },
    checkAllTodos(e){
      this.todos.forEach((todo) => todo.completed = e.target.checked)
    }
  },
};
</script>

<style scoped lang="scss">
.todo-app {
  text-align: left;
  width: 60%;
  margin: auto;

  .todo-input {
    width: 100%;
    padding: 6px;
    margin-bottom: 15px;
    font-size: 16px;
    border: 1px solid #c4c0c0;
    &:focus {
      outline: 0;
    }
  }
  .todo-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 0;
    .todo-item-left {
      display: flex;
    }
    .completed {
      text-decoration: line-through;
    }
    .todo-title {
      margin-bottom: 5px;
      width: 16vw;
    }
  }
  .remove-item {
    cursor: pointer;
  }
  .extra-container{
    display: flex;
    align-items: center ;
    justify-content: space-between;
    border-top: 1px solid gray;
    font-size: 16px;
    padding-top: 14px;
    margin-bottom: 14px;
  }
}
</style>
