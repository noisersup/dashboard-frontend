<template>
  <div id="widget">
    <div class="header">
      <h2>TODO</h2>
    </div>
      <div class="wrapper">
        <div class="input-data">
          <input type="text" class="todo-input" v-model="taskInput" @keyup.enter="addTask" required>
          <div class="underline"></div>
          <label>Task</label>
        </div>
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
      tasks: ["","","",""],
      doneTasks: [{'title':"aaa"}]
    }
  },
  methods: {
      syncTasks(){
          (async () => {
            const res = await fetch("http://dashboard.com/api/todo/tasks");
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
          const res = await fetch("http://dashboard.com/api/todo/tasks", {
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
          const res = await fetch("http://dashboard.com/api/todo/tasks/"+id+"/", requestOptions)
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
          const res = await fetch("http://dashboard.com/api/todo/tasks/"+id+"/", requestOptions)
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
        background: rgb(30,255,0);
        background: linear-gradient(135deg, rgba(30,255,0,1) 0%, rgba(209,255,210,1) 100%);
        overflow: hidden;
        overflow-y: scroll;
        scrollbar-width: none;
    }
    #widget > *{
        height:40px;
        border-bottom: 1px solid black;
    }
    .header{
      border:none !important;
      height: 50px !important;
    }
    .header * {
      margin: 0;
    }
    .header h2 {
      padding: 15px 0px 0px 15px;
      text-align: left;
    }
    .task{
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-left: 8px;
        transition: 0.3s;
    }
    
    .task:hover{
      background-color: #fff;
    }

    .wrapper{
      background: #fff;
      border: none !important;
      padding: 20px 10px 5px 10px;
      box-shadow: 0px 0px 10px rgba(0,0,0,0.1);
    }

    .wrapper .input-data{
      height: 20px;
      width: 100%;
      position: relative;
    }

    .wrapper .input-data input{
      height: 100%;
      width: 100%;
      border: none;
      font-size: 17px;
      border-bottom: 2px solid silver;
    }

    .wrapper .input-data label{
      position: absolute;
      bottom: 0px;
      left:0;
      color: grey;
      pointer-events: none;
      transition: all 0.3s ease;
    }

    .input-data input:focus ~ label,
    .input-data input:valid ~ label {
      transform: translateY(-20px);
      font-size: 15px;
    }


    .wrapper .input-data .underline{
      position:absolute;
      bottom: -4px;
      left: -1px;
      height: 2px;
      width:105%;
    }


    .input-data .underline:before{
      position:absolute;
      content: "";
      height: 100%;
      background-color: rgb(178, 255, 143);
      width:100%;
      transform: scaleX(0);
      left: 0;
      transition: transform 0.3s ease;
    }
    input:focus{
      outline-width: 0;
          outline: none;
    }

    .input-data input:focus ~ .underline:before,
    .input-data input:valid ~  .underline:before{
        transform: scaleX(1);
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
      background: rgba(128, 126, 126,0.6);
    }
    .done p {        
      text-decoration: line-through;
    }
    .removeBtn:hover{
      background-color: rgb(255, 87, 87);
    }
</style>
