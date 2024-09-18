<template>
  <section class="flex items-center justify-center">
    <div class=" w-full max-w-xl mb-96 px-4 sm:px-6 lg:px-8">
      <div id="pop-up" v-bind:style="popupStyles">{{ popupMessage }}</div>
      <div id="container mb-12">
        <h1 class="text-center">
          ChafiTask 
          <i class="fa fa-plus" aria-hidden="true" @click="toggleInput"></i>
        </h1>
        <input 
          v-if="showInput" 
          type="text" 
          v-model="newTodo" 
          @keyup.enter="addTodo" 
          placeholder="Inserez une tâche"
          class="w-full p-2 border border-gray-300 rounded"
        />
        <ul id="ullist" class="mt-6">
          <li 
            class="ml-6 mb-2"
            v-for="(todo, index) in todos" 
            :key="index" 
            :style="getTodoStyle(index)">
            <span class="delete" @click="removeTodo(index)">
              <i class="fa fa-trash" aria-hidden="true"></i>
            </span>
            <span :class="{ marked: todo.completed }" @click="toggleComplete(index)">
              {{ todo.text }}
            </span>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>
<style scoped>
* {
  transition: 0.5s;
}
body {
  background: #485563;
  background: -webkit-linear-gradient(to left, #485563, #29323c);
  background: linear-gradient(to left, #485563, #29323c);
}
h1 {
  border-radius: 10px 10px 0 0;
  font-family: 'Titillium Web', sans-serif;
  text-align: center;
  background: #272727;
  color: #fff;
  margin: 0;
  padding: 10px;
  font-weight: normal;
  box-shadow: inset 0px 32px 50px -31px rgba(255, 255, 255, 0.10);
}
.marked {
  color: gray;
  text-decoration: line-through;
}
.fa-plus {
  float: right;
  line-height: 50px;
}
.fa-plus:hover {
  text-shadow: 0 0 4px #fff;
}
.delete {
  transition: 0.5s linear;
  color: #fff;
  text-align: center;
  background: #777;
  width: 5px;
  display: inline-block;
}
li:hover .delete {
  opacity: 1.0;
  width: 40px;
}
.fa-trash {
  transition: 0.3s;
  opacity: 0;
}
li:hover .fa-trash {
  opacity: 1.0;
  transition: 0.2s linear;
}
#pop-up {
  font-family: 'Titillium Web', sans-serif;
  text-transform: uppercase;
  color: #fff;
  text-align: center;
  background: #666;
  width: 200px;
  position: absolute;
  margin: 10px;
  padding: 20px 0 0 0;
  border-radius: 0 0 10px 10px;
  opacity: 0;
  box-shadow: 0 5px 10px #333;
  background: linear-gradient(to left, #485563, #29323c);
}
#container {
  margin: 0 auto;
  width: 100%;
  max-width: 500px;
  min-width: 200px;
}
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
li {
  font-family: 'Quicksand', sans-serif;
  list-style-type: none;
  line-height: 50px;
  color: #fff;
  box-shadow: inset 0px -32px 100px -31px rgba(0, 0, 0, 0.08);
  word-wrap: break-word;
}
li:hover {
  text-shadow: 0 3px 2px rgba(0, 0, 0, 0.5);
  font-size: 110%;
  border-left: 3px solid #fff;
  border-right: 3px solid #fff;
  text-shadow: 0 0 4px #fff;
}
input {
  background: #333;
  color: #fff;
  text-transform: uppercase;
  font-family: 'Quicksand', sans-serif;
  box-sizing: border-box;
  width: 100%;
  padding: 13px;
  border: none;
  border: 3px solid #333;
  transition: 1.0s;
}
input:focus {
  border: 3px solid #272727;
  outline: none;
  transition: 0s;
  text-shadow: 0 0 5px #fff;
}
</style>
<script>
export default {
  data() {
    name: 'Task'
    return {
      todos: [],
      newTodo: "",
      showInput: true,
      popupMessage: "",
      popuptoggled: false,
    };
  },
  computed: {
    popupStyles() {
      return {
        opacity: this.popuptoggled ? "1" : "0",
        height: this.popuptoggled ? "50px" : "0",
        color: this.popupMessage === "Tâche ajouté" ? "#FFF" : "yellow"
      };
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === "") {
        this.showPopup("Joan le batard!", "warning");
        return;
      }
      this.todos.push({ text: this.newTodo, completed: false });
      this.newTodo = "";
      this.showPopup("Tâche ajouté man !", "added");
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleComplete(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
    toggleInput() {
      this.showInput = !this.showInput;
    },
    showPopup(message, type) {
      this.popupMessage = message;
      this.popuptoggled = true;
      setTimeout(() => {
        this.popuptoggled = false;
      }, 4000);
    },
    getTodoStyle(index) {
      // Color logic based on the index
      let r = 72 + (index * 10),
          g = 99 + (index * 5),
          b = 85 + (index * 10),
          a = 0.5;
      return `background: rgba(${r}, ${g}, ${b}, ${a});`;
    }
  }
};
  
</script>