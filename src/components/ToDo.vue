<template>
  <div class="toDo">
    <h1>{{ msg }}</h1>
    <div class="form">
      <label>Nom</label>
      <input class="input" type="text" v-model="newTodoName">
      <label>Nombre d'heures</label>
      <input input class="input" type="number" v-model="newTodoNbHours">
      <label>Responsable</label>
      <select name="responsible" id="responsible" v-model="newTodoResponsible">
        <option value="Pierre">Pierre</option>
        <option value="Paul">Paul</option>
        <option value="Jacques">Jacques</option>
      </select>
      <button @click="addTodo()">+</button>
    </div>
    <div class="toDoList">
      <div class="element" v-for="(todo, index) in todos" :key="index">
        <a @click="select(index)">
            {{ todo.name }} /
            {{ todo.nbHours }}h /
            {{ todo.responsible }}
        </a>
          <button @click="changeStatus(index)" class="btnChangeStatus">{{ todo.status }}</button>            
          <button class="btnUpdate" @click="editTodo(index)">Modifier</button>
          <button class="btnDelete" @click="deleteTodo(index)">Supprimer</button>
      </div>
    </div>
    <p>Il y a {{ todos.length }} tâches.<br />Dont {{ nbSelected }} sélectionnée(s)</p>
    <button class="btnDelete" @click="deleteTodosSelect()">Supprimer les tâches sélectionnées</button>
  </div>
</template>

<script>
export default {
  name: 'ToDo',
  props: {
    msg: String
  },
  data() {
    return {
      newTodoName: "",
      indexEditTodo: null,
      todoStatus: ["to-do", "on-going", "finished"],
      todos: [],
      todosSelected: [],
      nbSelected: 0,
    };
  },

  methods: {
    addTodo() {
      if (!this.newTodoName || !this.newTodoNbHours) {
        alert('Champs non saisi ou nombre d\'heures incorrect');
        return;
      }
        
      if (this.indexEditTodo === null) {
        this.todos.push({
          name: this.newTodoName,
          nbHours: this.newTodoNbHours,
          responsible: this.newTodoResponsible,
          status: "to-do",
        });
      } else {
        this.todos[this.indexEditTodo].name = this.newTodoName;
        this.todos[this.indexEditTodo].nbHours = this.newTodoNbHours;
        this.todos[this.indexEditTodo].responsible = this.newTodoResponsible;
        this.indexEditTodo = null;
      }
      this.newTodoName = "";
      this.newTodoNbHours = "";
    },
    editTodo(index) {
      this.newTodoName = this.todos[index].name;
      this.newTodoNbHours = this.todos[index].nbHours;
      this.newTodoResponsible = this.todos[index].responsible;
      this.indexEditTodo = index;
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    deleteTodosSelect() {
      this.todosSelected.forEach(element => {
        this.todos.splice(element, 1);
      });
      this.nbSelected=0;
    },
    changeStatus(index) {
      let statusIndex = this.todoStatus.indexOf(this.todos[index].status);
      if (++statusIndex > 2) statusIndex = 0;
      this.todos[index].status = this.todoStatus[statusIndex];
    },
    select(index) {
      this.nbSelected++;
      this.todosSelected.push({index});
      index.classList.add("select");
    }
  }
}
</script>

<style scoped>
h1 {
  color: gray;
}
input, select {
  margin-right: 30px;
  border-radius: 50px;
}
label {
  margin-right: 10px;
}
button {
  margin: 0 10px;
  border-radius: 30px;
  background-color: gray;
  border: none;
  color: white;
}
.btnDelete {
  background-color: orange;
}
.btnChangeStatus {
  background-color: transparent;
}
.form, .toDoList{
  margin: 50px 10%;
  padding: 10px;
  background-color: beige;
  border-radius: 10px;
}
.toDoList {
  margin: 50px 30%;
  background-color: blanchedalmond;
  text-align: left
}
p {
  margin-top: 30px;
}
.element {
  margin: 5px;
  border-radius: 10px;
}
.select {
  background-color: gray;
}

</style>
