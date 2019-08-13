<template>
  <div id="app" class="container my-3">
  <div class="input-group mb-3">
    <div class="input-group-prepend">
      <span class="input-group-text" id="basic-addon1">待辦事項</span>
    </div>
    <input type="text" class="form-control" placeholder="準備要做的任務" @keyup.enter="addTodo()" v-model="newTodo">
    <div class="input-group-append">
      <button class="btn btn-primary" type="button" @click="addTodo()">新增</button>
    </div>
  </div>
  <div class="card text-center">
    <div class="card-header">
      <ul class="nav nav-tabs card-header-tabs">
        <li class="nav-item">
          <a class="nav-link" :class="{'active': visibility== 'all'}" @click="visibility = 'all'" href="#">全部</a>
        </li>
        <li class="nav-item">
          <a class="nav-link " :class="{'active': visibility== 'active'}" @click="visibility = 'active'" href="#">進行中</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" :class="{'active': visibility== 'completed'}" @click="visibility = 'completed'" href="#">已完成</a>
        </li>
      </ul>
    </div>
    <ul class="list-group list-group-flush text-left">
      <li class="list-group-item" v-for="item in filteredTodos" @dblclick="editTodo(item)">
        <div class="d-flex" v-if="item.id !== cacheTodo.id">
          <div class="form-check">
            <input type="checkbox" class="form-check-input" v-model="item.completed" :id="item.id">
            <label class="form-check-label" 
            :class="{'completed': item.completed}"
            :for="item.id">
              {{item.title}}
            </label>
          </div>
          <button type="button" class="close ml-auto" aria-label="Close" @click="removeTodo(item)">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>

         <input type="text" class="form-control" 
         v-model="cacheTitle"
         @keyup.esc="cancelEdit"
         @keyup.enter="doneEdit(item)"
         v-if="item.id === cacheTodo.id">

      </li>
      
      <!-- <li class="list-group-item">
        <input type="text" class="form-control">
      </li> -->

    </ul>
    <div class="card-footer d-flex justify-content-between">
      <span>還有 3 筆任務未完成</span>
      <a href="#">清除所有任務</a>
    </div>
  </div>
</div>
</template>

<script>
export default {
    
    data () {
        return {
                newTodo: '',
                todos:[
                    {
                        id: '1',
                        title:'這是一個很長很長故事',
                        completed: false
                    },
                    {
                        id: '2',
                        title:'無敵飄髮哥',
                        completed: false
                    },
                ],
                cacheTodo:{},
                cacheTitle: '',
                visibility: 'all'
            }
        },
    methods:{
        addTodo(){
            var value = this.newTodo.trim()
            var timestamp = Math.floor(Date.now())
            console.log(value, timestamp)

            this.todos.push({
                title: value,
                id: timestamp
            })
            this.newTodo=''
        },
        removeTodo(todo){
            var newIndex ='';
            var vm = this;
            vm.todos.forEach(function(item, key){
                if(todo.id === item.id){
                    newIndex = key
                }
            })
            vm.todos.splice(newIndex, 1)
        },
        editTodo(item){
            this.cacheTodo = item;
            this.cacheTitle = item.title

        },
        cancelEdit(){
          this.cacheTodo = {}
        },
        doneEdit(item){
          item.title = this.cacheTitle;
          this.cacheTitle = '';
          this.cacheTodo = {}
        }
    },
    computed:{
      filteredTodos(){
        if(this.visibility == 'all'){
           return this.todos;
        }else if(this.visibility == 'active'){
            var newTodos = [];
            this.todos.forEach(function(item){
              if(!item.completed){
                newTodos.push(item)
              }
            })
            return newTodos
        }else if(this.visibility == 'completed'){
            var newTodos = [];
            this.todos.forEach(function(item){
              if(item.completed){
                newTodos.push(item)
              }
            })
            return newTodos
        }
       
      }

    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .completed {
        text-decoration: line-through
        }
</style>
