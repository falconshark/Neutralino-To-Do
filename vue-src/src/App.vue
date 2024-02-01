<template>
  <main>
    <div class="header">
      <h1>Neutalino To Do List</h1>
    </div>
    <div class="to-do-list" v-if="dataReady">
      <div class="to-do-empty" v-if="lists.length === 0">
        Oh! nothing here. add something ?
      </div>
      <div class="to-do-contents" v-else>
        <ul>
          <li v-for="todo in lists" :class="{'completed': todo.completed}">
            <div class="to-do-text" v-if="editTarget !== todo.id">
              {{ todo.text }}
            </div>
            
            <div class="to-do-text-edit" v-else>
              <input v-model="todo.text"/>
              <div class="todo-save-button" @click="saveToDo()">
                <i class="bi bi-save"></i>
              </div>
            </div>
            
            <div class="todo-edit-button" @click="editToDo(todo)" 
            v-if="editTarget !== todo.id && !todo.completed">
              <i class="bi bi-pencil"></i>
            </div>

            <div class="todo-complete-button" @click="completeToDo(todo)" v-if="editTarget !== todo.id && !todo.completed">
              <i class="bi bi-check-lg"></i>
            </div>

            <div class="todo-remove-button" @click="removeTodo(todo)" v-if="editTarget !== todo.id">
              <i class="bi bi-trash"></i>
            </div>
          </li>
        </ul>
      </div>
      <div class="action-buttons">
        <button class="btn btn-primary btn-add" @click="addToDo">Add</button>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  data(){
    return{
      lists: [],
      selected: [],
      editTarget: null,
      dataReady: false,
    }
  },
  async mounted(){
    let savedList = null;
    if(typeof Neutralino === 'undefined'){
      savedList = window.localStorage.getItem('todo');
    }else{
      savedList = await Neutralino.storage.getData('todo');
    }
    if(savedList){
      savedList = JSON.parse(savedList);
      this.lists = savedList;
    }
    this.dataReady = true;
  },
  methods:{
    async saveContent(){
      if(typeof Neutralino === 'undefined'){
        localStorage.setItem('todo', JSON.stringify(this.lists));
      }else{
        await Neutralino.storage.setData('todo', JSON.stringify(this.lists));
      }
    },
    completeToDo(todo){
      const targetIndex = this.lists.findIndex((target) => {
        return todo.id == target.id;
      });
      this.lists[targetIndex].completed = true;
      this.saveContent();
    },
    removeTodo(todo){
      const newList = this.lists.filter((target) => {
        return todo.id !== target.id;
      });
      this.lists = newList;
      this.saveContent();
    },
    editToDo(todo){
      this.editTarget = todo.id;
    },
    saveToDo(){
      this.editTarget = null;
    },
    async addToDo(){
      const index = this.lists.length + 1;
      const text = `New todo ${index}`;
      const newToDo = {
        id: index,
        text,
        completed: false,
      };
      this.lists.push(newToDo);
      this.saveContent();
    },
  },
}
</script>

<style lang="scss">
body{
  font-size: 20px;
}
#app{
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.header{
  margin-bottom: 30px;
}
.to-do-contents{
  ul{
    padding-left: 0;
    list-style: none;
  }
  li{
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    &::before{
      content: '';
      border-radius: 50%;
      width: 10px;
      height: 10px;
      background-color: black;
      margin-right: 20px;
    }
    &:hover{
      .todo-edit-button, .todo-complete-button, .todo-remove-button{
        display: block;
      } 
    }

    &.completed{
      text-decoration:line-through;
    }

    .to-do-text{
      &:hover{
        cursor: pointer;
      }
    }
    input{
      border: 0;
      border-bottom: 1px solid;
      padding-left: 5px;
      border-radius: 0;
    }
    .todo-edit-button{
      margin-left: 20px;
      display: none;
      &:hover{
        cursor: pointer;
      }
    }
    .todo-complete-button{
      margin-left: 15px;
      display: none;
      &:hover{
        cursor: pointer;
      }
    }
    .todo-remove-button{
      margin-left: 15px;
      display: none;
      &:hover{
        cursor: pointer;
      }
    }
    .to-do-text-edit{
      display: flex;
      &:hover{
        cursor: pointer;
      }
      .bi{
        margin-left: 20px;
      }
    }
  }
}
.action-buttons{
  margin-top: 30px;
}
</style>
