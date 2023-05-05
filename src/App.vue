<template>
  <div class="container">
      <div class="max-w-sm rounded overflow-hidden shadow-lg m-0 m-auto mt-3">
          <h1 class="text-xl font-bold p-5">
              To Do List
          </h1>
         <div class="px-5">
              <form>
              <div class="flex py-2">
                  <label class="relative block w-full">
                      <input v-model="text" class="block bg-white border w-full border-slate-300 rounded-md py-2 pl-9 pr-3 shadow-sm focus:outline-none focus:border-sky-500 focus:ring-sky-500 focus:ring-1" placeholder="Add to do..." type="text" name="search"/>
                  </label>
                  <button type="button" @click="addTask" class="text-white bg-sky-500 hover:bg-sky-500 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-sky-500 dark:hover:bg-sky-600 focus:outline-none dark:focus:sky-700 ml-3">
                      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                          <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
                        </svg>
                  </button>
              </div>
              </form>
              <div class="flex py-2">
                  <button type="button" @click="active = null" class="text-sky-500 hover:text-white border border-sky-500 hover:bg-sky-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-11 py-2.5 text-center mr-2 mb-2 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-600 dark:focus:sky-blue-800">All</button>
                  <button type="button" @click="active = false" class="text-sky-500 hover:text-white border border-sky-500 hover:bg-sky-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-9 py-2.5 text-center mr-2 mb-2 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-600 dark:focus:sky-blue-800">Active</button>
                  <button type="button" @click="active = true" class="text-sky-500 hover:text-white border border-sky-500 hover:bg-sky-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-600 dark:focus:sky-blue-800">Completed</button>
              </div>
              <div class="py-2" v-if="allTasks.length">
                  <ul class="w-full" v-for="task in allTasks" :key="task.id">
                    <li class="bg-gray-200 px-3 py-3 my-2 flex justify-between" v-show="active === null || active === task.completed">
                      <div class="">
                        <input type="checkbox" class="w-4 h-4 mr-2" :checked="task.completed" @click="completeTask($event,task.id)">
                              <label for="" :class="{'line-through':task.completed}">{{ task.name }}</label>
                        </div>
                      <button type="button" class="" @click="deleteTask(task.id)">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 stroke-red-500">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
                              </svg>  
                        </button>
                    </li>
                  </ul>
              </div>
              <div class="py-1">
                  <label for="" class="text-gray-500" id="item-count"></label>
              </div>
              <div class="flex justify-between py-2">
                  <button type="button" @click="toogleCheck" id="check" class="text-sky-500 hover:text-white border border-sky-500 hover:bg-sky-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-10 py-2.5 text-center mr-2 mb-2 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-600 dark:focus:sky-blue-800">{{ !completed ? "CheckAll" : "UnCheckAll" }}</button>
                  <button type="button" @click="clearComplete" class="text-sky-500 hover:text-white border border-sky-500 hover:bg-sky-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-7 py-2.5 text-center mr-2 mb-2 dark:border-blue-500 dark:text-blue-500 dark:hover:text-white dark:hover:bg-blue-600 dark:focus:sky-blue-800">Clear Completed</button>
              </div>
          </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
  return {
    text: '',
    allTasks : JSON.parse(localStorage.getItem("tasks")) || [],
    active : null,
    completed : false
  }},
  methods:{
    addTask(){
        if (this.text != ''){
           this.allTasks = [...this.allTasks,{ id: new Date().getTime(), name: this.text, completed: false }]
        }
          this.text = '';
    },
    completeTask(e,index){
      let updateTasks = this.allTasks.map((task)=>{
        if(task.id == index){
          task.completed = e.target.checked;
        };
        return task;
      })
      this.allTasks = updateTasks;
    },
    deleteTask(id){
      let filterTasks = this.allTasks.filter((task)=>{
        return task.id != id;
      })
      this.allTasks = filterTasks;
    },
    toogleCheck(){
      let filterTasks = this.allTasks.filter((task)=>{
        return task.completed  == true;
      })
      if(filterTasks.length == 0){
            this.allTasks = this.allTasks.map(task => {
            task.completed = true
            this.completed = true
            return task;
          });
      }
      else{
          this.allTasks = this.allTasks.map(task => {
          task.completed = false
          this.completed = false
          return task;
        });
      }
    },
      clearComplete(){
          this.allTasks = this.allTasks.filter((task)=>{
          return task.completed  == false;
        })
    }
  },
  watch:{
    allTasks:function(newValue) {
      localStorage.setItem('tasks', JSON.stringify(newValue))
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
 