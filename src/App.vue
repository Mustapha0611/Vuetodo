<template>
  <div id="app" :class="{ lightgb: lightMode }">
    <div class="container">
      <div class="one" :class="{ light: lightMode }"></div>
      <div class="two" :class="{ bglight: lightMode }">
        <!-- <img src="@/assets/icon-check.svg" alt="" srcset=""> -->
      </div>
    </div>
    <div class="main">
      <div class="heading">
        <h1>TODO</h1>
        <span class="mode-changer" @click="toggleMode">
          <img
            src="@/assets/icon-sun.svg"
            alt=""
            class="bg-changer"
            v-if="darkMode"
          />
          <img
            src="@/assets/icon-moon.svg"
            alt=""
            class="bg-changer"
            v-if="lightMode"
          />
        </span>
      </div>
      <div class="input-field">
        <input
          type="text"
          v-model="textInput"
          placeholder="Create a new todo..."
          class="input-text"
          :class="{ inputlight: lightMode }"
          @keyup.enter="addTodo"
        />
      </div>
      <ul :class="{ listbg: lightMode }" v-if="todos.length > 0">
        <li v-for="(todo, id) in display" :key="todo.id">
          <input type="checkbox" v-model="todo.isDone" id="checked" />
          <p :class="{ strike: todo.isDone }">{{ todo.todo }}</p>
          <div @click="removeTodo(todo.id)">
            <img src="@/assets/icon-cross.svg" alt="" srcset="" />
          </div>
        </li>
        <div class="bottom">
          <span>{{ remaining }} items left</span>
          <section class="filter">
            <span @click="filter">All</span>
            <span @click="filter">Active</span>
            <span @click="filter">Completed</span>
          </section>
          <span @click="clearCompleted" class="clear-completed"
            >Clear Completed</span
          >
        </div>
      </ul>
      <div
        class="addtodo"
        :class="{ ligthbg: lightMode }"
        v-if="todos.length == 0"
      >
        No Tasks Available
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      darkMode: true,
      lightMode: false,
      textInput: "",
      filterItem: "All",
      idinc:0,
      display: [],
      todos: []
    };
  },
  methods: {
    toggleMode() {
      this.darkMode = !this.darkMode;
      this.lightMode = !this.lightMode;
    },
    addTodo() {
      if (this.textInput == "") {
        console.log("error");
      } else {
        this.todos.push({
          id: ++this.idinc,
          todo: this.textInput,
          isDone: false,
        });
        this.textInput = "";
      }
    },
    removeTodo(id) {
      this.todos = this.todos.filter(todo => todo.id!== id);
      this.filteredTodos();
    },
    filter(e) {
      this.filteritem = e.target.innerText;
      this.filteredTodos();
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.isDone);
      this.display = this.todos;
    },

    filteredTodos() {
      if (this.filteritem === "All") {
        this.display = this.todos;
      } else if (this.filteritem === "Active") {
        this.display = this.todos.filter((todo) => !todo.isDone);
      } else if (this.filteritem === "Completed") {
        this.display = this.todos.filter((todo) => todo.isDone);
      }
      return this.display;
    },
  },
  mounted() {
    this.display = this.todos;
    this.filteredTodos();
  },
  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.isDone).length;
    }
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Lexend+Deca&display=swap");
* {
  margin: 0;
  padding: 0;
  font-family: "Lexend Deca", sans-serif;
}
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  background-color: hsl(235, 21%, 11%);
}
.container {
  height: 100vh;
  width: 100%;
}
.one {
  height: 40%;
  width: 100%;
  background-image: url("@/assets/bg-desktop-dark.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  transition: all 0.3s;
}
.light {
  background-image: url("@/assets/bg-desktop-light.jpg");
}
.two {
  height: 60%;
  min-height: 400px;
  width: 100%;
  background-color: hsl(235, 21%, 11%);
  transition: all 0.3s;
}
.bglight {
  background: hsl(0, 0%, 98%);
  color: hsl(236, 9%, 61%);
}
.main {
  width: 600px;
  height: 50vh;
  position: absolute;
  top: 35%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.heading > h1 {
  font-size: 3rem;
  letter-spacing: 10px;
  font-weight: 500;
}
.bg-changer {
  position: absolute;
  right: 10px;
  top: 10px;
  cursor: pointer;
}
.input-field {
  margin-top: 40px;
}
.input-text{
  width: 93%;
  border: none;
  font-size: 18px;
  border-radius: 7px;
  padding: 20px 15px;
  background-color: hsl(235, 24%, 19%);
  color: #fff;
  outline: none;
}
.inputlight{
  color: hsl(235, 24%, 19%);
  background-color: #fff;
}
ul {
  list-style: none;
  background-color: hsl(235, 24%, 19%);
  margin-top: 30px;
  width: 98%;
  max-height: 60vh;
  overflow-y: scroll;
  border-radius: 7px;
}
::-webkit-scrollbar {
  width: 5px;
  height: 5px;
}

::-webkit-scrollbar-thumb {
  background-color: hsl(235, 20%, 54%);
}
.listbg {
  background-color: hsl(0, 0%, 100%);
  box-shadow: 1px 1px 2px 3px hsl(233, 100%, 97%);
  color: hsl(233, 14%, 35%);
}
li {
  overflow: hidden;
  padding: 20px 10px;
  border-bottom: 1px solid hsl(233, 14%, 35%);
  display: grid;
  grid-template-columns: 1fr 20fr 1fr;
}
li > p {
  padding: 0 10px;
  font-size: 16px;
  text-align: left;
  display: inline-block;
}
.strike {
  text-decoration: line-through;
  color: hsl(233, 14%, 35%);
}
li > div {
  display: inline-block;
  float: right;
  cursor: pointer;
}
.bottom {
  display: flex;
  justify-content: space-between;
  color: hsl(233, 14%, 35%);
  padding: 15px;
}
.filter span {
  margin: 0 10px;
}
span {
  cursor: pointer;
  /* color:hsl(233, 14%, 35%) ; */
  font-size: 14px;
  transition: all 0.2s;
}
span:hover {
  color: hsl(220, 98%, 61%);
}
#checked {
  appearance: none;
  -webkit-appearance: none;
  height: 18px;
  border-radius: 50%;
  width: 18px;
  border: 1px solid hsl(233, 14%, 35%);
  cursor: pointer;
  background: hsl(189, 28%, 14%);
}
#checked::after {
  content: "\2713";
  color: white;
  margin-left: 4px;
  margin-bottom: 4px;
  font-size: 10px;
  display: none;
  background-image: linear-gradient to right hsl(192, 100%, 67%)
    hsl(280, 87%, 65%);
}
#checked:checked {
  background: linear-gradient hsl(192, 100%, 67%) to hsl(280, 87%, 65%);
}
#checked:hover {
  border: 1px solid aqua;
}
#checked:checked::after {
  display: block;
}
.addtodo {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 50px 0;
  border-radius: 7px;
  width: 97%;
  font-size: 30px;
  height: 20vh;
  background-color: hsl(235, 24%, 19%);
  color: hsl(235, 20%, 54%);
}
.lightbg {
  background-color: hsl(0, 0%, 98%);
  color: hsl(236, 9%, 61%);
}
@media screen and (max-width: 650px) {
  #app {
    background-color: hsl(235, 21%, 11%);
  }
  #app .lightgb {
    background-color: hsl(0, 0%, 100%);
  }
  .one {
    background-image: url("@/assets/bg-mobile-dark.jpg");
  }
  .two {
    height: 400px;
    min-height: 300px;
    max-height: 1000px;
  }
  .light {
    background-image: url("@/assets/bg-mobile-light.jpg");
  }
  .main {
    width: 90%;
  }
  .heading > h1 {
    font-size: 2rem;
  }
  input[type="text"] {
    width: 91%;
  }
  ul {
    width: 100%;
    margin-bottom: 50px;
  }
  li > span {
    right: 20px;
  }
  .bottom {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 10px;
  }
  .bottom > span {
    order: 1;
  }
  .clear-completed {
    text-align: right;
  }
  .filter {
    order: 2;
    /* border:1px solid red; */
    grid-column-start: 1;
    grid-column-end: span 2;
    grid-row-start: 2;
    text-align: center;
    grid-row-end: 3;
  }
  .filter span {
    margin: 20px 5px;
  }
}
</style>
