<template>
    <div class="container" style="max-width: 600px">
        <h2 class="text-center mt-5">To-Do APP</h2>
        <div class="d-flex mt-5">
            <input type="text" v-model="task" placeholder="Enter Task..." 
            class="w-100 form control" />
            <button class="btn btn-primary rounded-0" @click="submitTask">Add Task</button>
        </div>
        <table class="table caption-top">
            <thead>
              <tr>
                <th scope="col">TASK</th>
                <th scope="col">STATUS</th>
                <th scope="col" class="text-center">DELETE</th>
                <th scope="col" class="text-center">EDIT</th>
              </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td><span :class="{ 'line-through': task.status === 'finished'}">{{task.name}}</span></td>
                    <td>
                        <span class="pointer noselect" @click="changeStatus(index)"
                        :class="{
                            'text-danger' : task.status === 'to-do',
                            'text-success' : task.status === 'finished',
                            'text-warning' : task.status === 'in-progress',
                        }">
                            {{ capitalizeFirstChar(task.status) }}
                        </span>
                    </td>
                    <td class="text-center pointer"><div @click="deleteTask(index)"><span>Del</span></div></td>
                    <td class="text-center pointer"><div @click="editTask(index)">Edit</div></td>
                </tr>
            </tbody>
          </table>
    </div>
</template>

<script>
export default{
    name: "TodoApp",

data(){
    return{
        task: "",
        editedTask: null,
        statuses: ["to-do", "in-progress", "finished"],
        tasks: [
            {
                name: "webcodecamp",
                status: "to-do"
            },        
            {
                name: "xyz",
                status: "in-progress"
            },        
            {
                name: "webcodecamp",
                status: "finished",
            },
            ],
        }
    },

methods: {
    capitalizeFirstChar(str){
        return str.charAt(0).toUpperCase() + str.slice(1);
    },
    changeStatus(index){
        let newIndex = this.statuses.indexOf(this.tasks[index].status);
        if (++newIndex > 2) newIndex = 0;
        this.tasks[index].status = this.statuses[newIndex];
    },

    deleteTask(index){
        this.tasks.splice(index,1); 
    },

    editTask(index){
        this.task = this.tasks[index].name;
        this.editedTask = index;
    },
    submitTask(){
        if (this.task.length === 0)  
        {
            return;
        }

        else if (this.editedTask != null){
            this.tasks[this.editedTask].name = this.task;
            this.editedTask = null;
        }else {
            this.tasks.push({
                name: this.task,
                status: "todo",
            }),
        this.task = "";}
    }
}
}
</script>

<style>
    .pointer{
        cursor: pointer;
    }
    .line-through{
        text-decoration: line-through;
    }
</style>