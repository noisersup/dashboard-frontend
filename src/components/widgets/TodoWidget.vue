<template>
  <div id="widget">
    <input type="text" class="todo-input" placeholder="Insert task..."
    v-model="taskInput" @keyup.enter="addTask">
    <div v-for="task in tasks" :key="task" class="task">
        {{task.title}}
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
      tasks: [
        {
          'id': 1,
          'title': 'task 1',
          'done': false,
        },
        {
          'id': 2,
          'title': 'task 2',
          'done': false,
        },{
          'id': 3,
          'title': 'task 3',
          'done': false,
        },
        {
          'id': 4,
          'title': 'task 4',
          'done': false,
        },
      ]
    }
  },
  methods: {
      syncTasks(){
          (async () => {
            const res = await fetch("http://localhost:8000/tasks");
            res.json().then(
              data => {
                  this.tasks=data.tasks
              }
            );
          })();
      },
      addTask(){
        if(this.taskInput.trim().length==0) return;
        (async () => {
          const res = await fetch("http://localhost:8000/tasks", {
            method: 'POST',
            mode: 'no-cors',
            cache: 'no-cache',
            credentials: 'same-origin',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({title: this.taskInput})
          });
        })();
        this.taskInput='';
        this.syncTasks();
      },
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
    #widget *{
        height:40px;
        border-bottom: 1px solid black;
        
    }
    .task{
        display: flex;
        justify-content: center;
        align-items: center;
        
    }
</style>
