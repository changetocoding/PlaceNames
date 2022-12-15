<template>
  <div>
  <div>
    <h1>List</h1>
  </div> 
  <!-- Input -->
  <div>
    <input v-model="task" type="text" placeholder="Insert a new task">
     <select required name="priority" v-model="priority">
        <option>1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
        <option>6</option>
        <option>7</option>
        <option>8</option>
        <option>9</option>
        <option>10</option>
    </select>
    <button @click="add">Add task</button>
    <br/><br/>
    
    <button>Order by priority</button>
  </div>
  <tr v-for="(task, index) in tasks" :key="index">
  <div    v-for="(textQuestion, index) in textQuestions" :key="index">

    <component :is="componentType" :textQuestion="textQuestion"></component>

  </div>
  <TextQuestion
      v-for="textQuestion in textQuestions"
      v-bind:textQuestion="textQuestion"
      :key="textQuestion.label"
    />
  
  <!-- Task Table -->
  <table>
  <thead>
    <tr>
      <th scope="col" style='width: 240px'>Task</th>
      <th scope="col" style='width: 120px'>Status</th>
      <th scope="col" style='width: 120px'>Edit</th>
      <th scope="col" style='width: 120px'>Delete</th>
      <th scope="col" style='width: 120px'>Priority</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>{{task.name}}</td>
      <td style="width: 120px"><span @click="changeStatus(index)">{{ Uppo(task.status) }}</span></td>
      <td>
        <div @click="editTask(index)">
          <span>*Edit*</span>
        </div>
      </td>
      <td>
        <div @click="deleteTask(index)">
          <span>X</span>
        </div>
      </td>
      <td>
        <div>
          <span>{{ (task.priority) }}</span>
        </div>
      </td>
    </tr>
   
  </tbody>
</table>
</div>
</template>
  
  


<script>
export default {
  data() {
    return {
      task:'',
      priority:'',
      editedTask: null,
      status: ['available', 'taken', 'completed'],
      tasks: [
        {
          name: 'Give a job please',
          status: 'available',
          priority: '10'
        },
        {
          name: 'First drink on me',
          status: 'taken',
          priority: '3'
        }
      ],
      textQuestions:[{
        
      }]
    },
  },
  
  methods: {
    add(){
      if(this.task.length == 0) return;      
      if(this.priority == 0) return;
      
      if(this.editedTask == null){
        this.tasks.push({
          name: this.task,
          status: 'available',
          priority: this.priority
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.tasks[this.editedTask].priority = this.priority;
        this.editedTask = null;
      }
      this.task = '';
      this.priority = null;
    },
    
    deleteTask(index){
      this.tasks.splice(index, 1);
    },
    
    editTask(index){
      this.task = this.tasks[index].name;      
      this.editedTask = index;
    },
    
    changeStatus(index){
      let newIndex = this.status.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.status[newIndex];
    },
    
    Uppo(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    },  
    

  },  
}
</script>
