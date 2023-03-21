<script setup>
import { ref,computed } from 'vue';

        const toDo = ref([
             {id:1, title:'Task 1', status:false},
             {id:2, title:'Task 2', status:false},
        ]);


        const newTask = ref('');
        const updateData = ref('');
        const toDoAsc=computed(()=>toDo.value && toDo.value.sort((a,b) => a.id > b.id ? 1 :-1))

        //Add Task
        const addTask = () => {
            const num=toDo.value.length+1
            if (newTask.value.trim() === '') {
		        return
            }
            toDo.value.push({
                id :num,
                title: newTask.value,
                status:false
            })
            newTask.value=''
               
        }

        //Delete Task 
        const deleteTask = (id) => {
            let newTasks = toDo.value.filter(task => task.id !== id)
            toDo.value=newTasks;
        }

        //Mark task completed 
        const markDone = (id) => {
            let newTask = toDo.value.map(task => {
                if(task.id === id ) {
                    return ({...task, status:!task.status})
                }
                return task;
            })
            toDo.value=newTask;
        }

     

        //dataUpdate
        const dataUpdate=(newData)=>{
           
            updateData.value=newData
            console.log(updateData.value,'data');
        }
        

         //CancelUpdate
        const cancelUpdate = () => {
            dataUpdate('')
        }
        //Change task for update
        const changeTask = () => {
           let newEntry = {
            id: updateData.value.id,
            title:updateData.value.title,
            status:updateData.status ? true : false
           }
          
           dataUpdate(newEntry)
         
        }


        //Update task
        const updateTask = () => {
        let filterRecords =toDo.value.filter(task => task.id !== updateData.value.id)
        let updatedObject =[...filterRecords,updateData.value]
        toDo.value=updatedObject;
        dataUpdate('')  
        } 
        
  
        // on enter
        const onEnter = () => {
            addTask();
        }      
 
</script>

<template>
    <div class="container">
        <div v-if="updateData">
        <!-- UPDATE TASK -->
            <div class="row">
                <div class="column">
                    <input v-model="updateData.title" type="text"  name="updateData.title" @change="changeTask" class="form_control" />
                </div>
                <div class="column_auto">
                    <button class="btn" @click="updateTask"> Update</button>
                    <button class="btn btn_cancel" @click="cancelUpdate"> Cancel</button>
                </div>
            </div>
        </div>
        <div v-else>
            <!-- ADD TASK -->
        <div class="row">
            <div>
                <input v-model="newTask" name="newTask" type="text" @key="onEnter" class="form_control" />
            </div>
            <div >
                <button @click.prevent="addTask"  class="btn">Add Task</button>
            </div>
        </div>
        <br/>  
        </div>
     
         <div class="task_view" >
            <div v-if="toDo && toDo.length"></div>
            <div v-else>No Task....</div>
            <div v-for="(task,index) in toDoAsc" :key="task.id">
                <div class=" col task_item">
                    <div :class="`${task.status ? 'done' : ''}`">
                        <span class="task_num">{{index+1}}</span>
                        <span class="task_text" >{{task.title}}</span>
                    </div>
                    <div class="task_icon">
                        <span @click="()=>markDone(task.id)">Completed</span>
                        <span v-if="task.status"></span>
                        <span v-else  @click="()=>dataUpdate({
                            id:task.id,
                            title:task.title,
                            status:task.status ? true : false
                        })">Edit</span>
                        <span @click="deleteTask(task.id)">Delete</span>
                    </div>
                </div>
            </div>
        </div> 
    </div>   
</template>
<style>
    .row {
        display: flex;
        justify-content: center;
        align-items: center;
        width:auto;
    }
    .form_control {
        width:500px;
        height:30px;
    }
    .btn {
        margin:10px;
        width:100px;
        height:35px;
        background-color: green;
        cursor: pointer;
        border:none;
    }
    .btn_cancel {
        background-color: yellow;
    }
    .task_view {
        text-align: center;
    }
    .col {
        padding: 10px;
    }
    .task_item {
        background-color: antiquewhite;
        margin-bottom: 20px;
        padding: 10px,140px,10px,15px;
        text-align: left;
        position: relative;
    }
    .task_item div {
        display: flex;
        align-items: center;
    }
    .task_num {
        padding: 10px;
        font-size:22px;
        display: flex;
        margin-right: 8px;
        align-items: center;
        justify-content: center;
    }
    .task_item .task_text {
        flex:1;
    }
    .task_item .done .task_text{
        text-decoration: line-through;
    }
    .task_text {
        padding: 10px;
        font-size: 22px;
    }
   
  
    .task_item .task_icon {
        position: absolute;
        right:15px;
        top:30%;
        width: auto;
        display: inline-block;
    }
    .task_item .task_icon span {
        display: inline-block;
        margin: 0px 10px 0 10px;
        cursor:pointer;
        color:green;
        font-size: 22px;
    }
    .task_item .task_icon span:hover {
        color:black;
    }
</style>