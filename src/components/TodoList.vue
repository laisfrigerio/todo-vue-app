<template>
  <div>
    <input v-model="newTodo" type="text" name="todo" id="todo-input" class="todo-input" placeholder="Whats needs to be done ..." @keyup.enter="addTodo"/>
    <div class="todos">
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
          <div>
            <input
              type="checkbox"
              v-model="todo.completed"
            >
          </div>
          <div
            v-if="!todo.editing"
            class="todo-item-label"
            @dblclick="editTodo(todo)"
            :class="{ completed : todo.completed }"
          >
            {{ todo.title }}
          </div>

          <input
            v-else
            class="todo-item-edit"
            type="text"
            v-model="todo.title"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            @keyup.esc="cancelEdit(todo)"
            v-focus
          >
        </div>
        <div class="remove-todo" @click="removeTodo(index)">
          &times;
        </div>
      </div>
    </div>
    <div class="extra-container">
      <div>
        <input
          type="checkbox"
          class="check-all"
          id="check-all"
          :checked="!anyRemaining"
          @change="checkAllTodos"
        >
        <label for="check-all">Check all</label>
      </div>
      <div>
        <button
          class="clear-completed"
          @click="clearAllChecked()"
        >
          Clear completed
        </button>
      </div>
      <div>{{ remaining }} items left</div>
    </div>
  </div>
</template>

<script>

  export default {
    name: 'todo-list',
    data() {
      return {
        newTodo: "",
        idForTodo: 3,
        beforeEditCache: "",
        todos: [
          {
            id: 1,
            title: "Learn Vue JS",
            completed: false,
            editing: false
          },
          {
            id: 2,
            title: "Finish React JS Udemy course",
            completed: false,
            editing: false
          }
        ]
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim().length === 0) {
          return;
        }

        this.todos.push({
          id: this.idForTodo++,
          title: this.newTodo,
          completed: false
        });
        this.newTodo="";
      },

      editTodo(todo) {
        this.beforeEditCache=todo.title;
        todo.editing=true
      },

      doneEdit(todo) {
        if (todo.title.trim().length === 0) {
          todo.title = this.beforeEditCache;
        }

        todo.editing=false
      },

      cancelEdit(todo) {
        todo.editing=false;
        todo.title=this.beforeEditCache;
        this.beforeEditCache="";
      },

      removeTodo(index) {
        this.todos.splice(index, 1);
      },

      checkAllTodos() {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
      },

      clearAllChecked() {
        this.todos.forEach((todo) => todo.completed = false)
      }
    },
    computed: {
      remaining() {
        return this.todos.filter(todo => !todo.completed).length;
      },
      anyRemaining() {
        return this.remaining !== 0
      }
    },
    directives: {
      focus: {
        inserted: function (el) {
          el.focus()
        }
      }
    }
  }

</script>

<style lang="scss">
  .todo-input {
    width: 100%;
    padding: 10px 10px;
    font-size: 18px;
    margin-bottom: 16px;

    &:focus {
      outline: 0;
    }
  }

  .todos {
    border-bottom: 1px solid #ccc;
  }

  .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .remove-todo {
    cursor: pointer;
    margin-left: 12px;
    &:hover {
      color: red;
    }
  }

  .todo-item-left {
    display: flex;
    align-items: center;
  }

  .todo-item-label {
    padding: 10px;
  }

  .todo-item-edit {
    margin-left: 12px;
    padding: 10px;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    border: 1px solid #ccc;
    width: 100%;

    &:focus {
      outline: 0;
    }
  }

  .completed {
    text-decoration: line-through;
    color: grey;
  }

  .extra-container {
    /*border-top: 1px solid #ccc;*/
    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .clear-completed {
    cursor: pointer;
    padding: 5px 5px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;

    &:hover {
      background-color: #f8f8f8;
    }

    &:focus {
      outline: 0;
    }
  }
</style>
