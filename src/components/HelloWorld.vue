<template>
  <div>
    <form class="register" @submit.prevent="handleTodoSubmit">
        <p>TodoList</p>
        <input type="text" id="register_text" placeholder="할일을 등록하세요." required>
        <button type="submit" id ="resiger_button">등록</button>
    </form>
    <form>
        <ul class="list"></ul>
    </form>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';


interface Todo {
  text: string,
  id: number,
  check: boolean
 
}
@Options({
  props: {
    msg: String
  }
})
export default class HelloWorld extends Vue {

saveTodos():void {
  localStorage.setItem(this.TODOS_KEY, JSON.stringify(this.todos));
}

insertTodo(newTodo: Todo) {
  const list:any = document.querySelector('.list');
  console.log(this.todos);
  const li = document.createElement('li');
  const input = document.createElement('input');
  const span = document.createElement('span');
  input.id = newTodo.id as unknown as string;
  input.addEventListener('click', this.checkEnd);

  if (newTodo.check) {
    input.checked = true;
    li.classList.toggle('checked');
  }

  span.innerText = newTodo.text;
  input.type = 'checkbox';
  li.appendChild(input);
  li.appendChild(span);
  list.prepend(li);
}

 checkEnd(event: MouseEvent) {
  let myInput = event.target as HTMLInputElement;

  for (let i = 0; i < this.todos.length; i++) {
    if (this.todos[i].id.toString() == myInput.id) {
      this.todos[i].check = myInput.checked;
      (myInput.parentNode as HTMLElement).classList.toggle('checked');
      break;
    }
  }
  this.saveTodos();
}

handleTodoSubmit() {
  const register_text:HTMLInputElement = document.querySelector('#register_text') as HTMLInputElement; 
  console.log(register_text);
  const newTodo: string = register_text?.value as string;
  const newTodoObj:Todo = {
    text:newTodo,
    id:Date.now(),
    check:false
  };
  (register_text as HTMLInputElement).value = '';
  this.todos.push(newTodoObj);
  this.insertTodo(newTodoObj);
  this.saveTodos();
}

TODOS_KEY = 'todos';
todos:Array<Todo> = [];
mounted(){
  let savedTodos = localStorage.getItem(this.TODOS_KEY);
  console.log(savedTodos)
if (savedTodos) {
  const parsedTodos = JSON.parse(savedTodos);
  this.todos = parsedTodos;
  this.todos.forEach(this.insertTodo);
}

}

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
    text-align: center;
  }
p{
    font-size: 40px;
    font-weight: bold;
    margin-top: 0px;
    margin-bottom: 20px;
    padding-bottom: 10px;
    padding-top: 10px;
    background-color: deeppink;
    color: white;
    border-radius: 20px 20px 0px 0px / 20px 20px 0px 0px;
}
 div{
    display: inline-block;
    border: 2px solid deeppink;
    border-radius: 20px 20px 20px 20px / 20px 20px 20px 20px;
    width: 550px;
    height: 700px;
    background-color: snow;
 }
  #register_text {
    width: 350px;
    height: 20px;
    border-color: deeppink;
  }
  button {
    width: 50px;
    height: 30px;
    border : none;
    background-color: deeppink;
    color: white;
    font-weight: bold;
    border-radius: 5px 5px 5px 5px / 5px 5px 5px 5px;
  }
  ul {
    list-style: none;
    text-align: left;
    margin-left: 10%;
    font-weight: bold;
    color:deeppink;
  }
  li {
    margin-bottom: 10px;
  }
  
  li.checked {
    font-weight: lighter;
    text-decoration: line-through;
    font-style: italic;
    color:gray
  }
</style>
