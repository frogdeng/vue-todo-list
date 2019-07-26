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
          <a class="nav-link active" href="#">全部</a>
        </li>
        <li class="nav-item">
          <a class="nav-link " href="#">進行中</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">已完成</a>
        </li>
      </ul>
    </div>
    <ul class="list-group list-group-flush text-left">
      <li class="list-group-item" v-for="item in todos">
        <div class="d-flex">
          <div class="form-check">
            <input type="checkbox" class="form-check-input" v-model="item.complated" :id="item.id">
            <label class="form-check-label" 
            :class="{'completed': item.complated}"
            :for="item.id">
              {{item.title}}
            </label>
          </div>
          <button type="button" class="close ml-auto" aria-label="Close" @click="removeTodo(item)">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
      </li>
      <!-- <li class="list-group-item">
        <div class="d-flex">
          <div class="form-check">
            <input type="checkbox" class="form-check-input" id="a1">
            <label class="form-check-label completed" for="a1">
              Cras justo odio
            </label>
          </div>
          <button type="button" class="close ml-auto" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
      </li> -->
      <li class="list-group-item">
        <input type="text" class="form-control">
      </li>
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
                        complated: false
                    },
                    {
                        id: '2',
                        title:'無敵飄髮哥',
                        complated: false
                    },
                ]
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
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .completed {
        text-decoration: line-through
        }
</style>
