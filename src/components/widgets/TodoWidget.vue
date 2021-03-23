<template>
  <div id="widget">
    <div>
      <input type="text" class="todo-input" placeholder="Insert task..." v-model="taskInput" @keyup.enter="addTask">
    </div>
    <div v-for="task in tasks" :key="task" class="task">
      <p @click="completeTask(task.id,true)">{{task.title}}</p>
      <div class="removeBtn" @click="removeTask(task.id)">
        <div>&times;</div>
      </div>
    </div>
    <div v-for="task in doneTasks" :key="task" class="task done">
      <p @click="completeTask(task.id,false)">{{task.title}}</p>
      <div class="removeBtn" @click="removeTask(task.id)">
        <div>&times;</div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'TodoWidget',
  data () {
    return {
      taskInput: '',
      tasks: [],
      doneTasks: []
    }
  },
  methods: {
      syncTasks(){
          (async () => {
            const res = await fetch("http://localhost:8000/tasks");
            res.json().then(
              data => {
                  let newTasks = [];
                  let newDoneTasks = [];
                  if(data.tasks != null){
                    data.tasks.forEach(task=>{
                      if(task.done) newDoneTasks.push(task);
                      else newTasks.push(task);
                    });
                  }
                  this.tasks = newTasks;
                  this.doneTasks = newDoneTasks;
              }
            );
          })();
      },
      addTask(){
        if(this.taskInput.trim().length==0) return;
        (async () => {
          const res = await fetch("http://localhost:8000/tasks", {
            method: 'POST',
            cache: 'no-cache',
            credentials: 'same-origin',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({title: this.taskInput})
          });
          this.syncTasks();
        })();
        this.taskInput='';
      },
      removeTask(id){
        var requestOptions = {
          method: 'DELETE',
          redirect: 'follow',
        };
        (async () => {
          const res = await fetch("http://localhost:8000/tasks/"+id, requestOptions)
          this.syncTasks();
        })();
      },
      completeTask(id,done){
        var requestOptions = {
          method: 'PATCH',
          redirect: 'follow',
          headers: {
              'Content-Type': 'application/json'
          },
          body: JSON.stringify({"done": done})
        };
        (async () => {
          const res = await fetch("http://localhost:8000/tasks/"+id, requestOptions)
          this.syncTasks();
        })();
      }
  },
  created: function(){this.syncTasks()}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    #widget{
        background-color: rgb(144, 243, 98);
        overflow: hidden;
        overflow-y: scroll;
        scrollbar-width: none;
    }
    #widget > *{
        height:40px;
        border-bottom: 1px solid black;
    }
    .task{
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-left: 8px;
        transition: 0.3s;
    }
    
    .task:hover{
      background-color: rgb(81, 255, 0);
    }
    .todo-input{
      height:20px !important;
    }
    .removeBtn{
      flex-shrink: 0;
      display: flex;
      width: 40px;
      height: 100%;
      justify-content: center;
      align-items: center;
    }
    .task > p{
        width: 100%;
        text-overflow: ellipsis;
        overflow: hidden;
    }
    .done{
      background-color: gray;
      
    }
    .removeBtn:hover{
      background-color: rgb(255, 87, 87);
    }
</style>
