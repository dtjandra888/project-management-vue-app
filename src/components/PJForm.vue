<template>
<div class="container">
    <form @submit.prevent="submit">
        <!--Title Input section-->
        <div class="input-group mb-3">
            <span class="input-group-text">Title</span>
            <input required class="form-control" v-model="title" >
            <button type="submit" class="btn btn-success">{{ isEditMode ? 'Save' : 'Add' }}</button>
        </div>

        <!--Description box section-->
        <div class="input-group" >
            <span class="input-group-text">Description</span>
            <textarea required class="form-control" aria-label="Description" v-model="description"></textarea>
        </div>

        <!--Tasks/list section-->
        <p style="margin-top: 10px;">Add Tasks</p>
        <form @submit.prevent="addTask">  
            <div class="input-group mb-3">
                <input v-model="taskName" type="text" class="form-control" placeholder="Task" aria-label="Recipient's username" aria-describedby="button-addon2">
                <button class="btn btn-outline-secondary" type="submit" id="button-addon2">+</button>
            </div>
        </form>
        <div>
            <div v-for="task in tasks" :key="task.id">
                <p>{{ task.name }}</p>
            </div>
        </div>
    </form>
</div>
</template>

<script>

let id = 0;
let taskId = 0;
export default{
    name: "ProjectForm",
    data(){
        return {
            title: '',
            description: '',
            tasks: [],
            taskName: '',
        };
    },

    props:{
        proj:{
            type: Object,
            default: () => ({})
        },
        isEditMode: {
            type: Boolean,
            default: false
        }
    },

    created() {
        if(this.isEditMode){
            this.title = this.proj.title
            this.description = this.proj.description
            this.tasks = this.proj.todoList
            return
        }
    },

    methods: {
        submit() {
            if(this.isEditMode){
                this.emitUpdateProj()
                return
            }
            this.emitNewProject()
        },
        addTask() {
            this.tasks.push({id: taskId++, name: this.taskName})
            this.taskName = ''
        },
        emitNewProject() {
            this.$emit('onNewProject', {
                id: id++,
                title: this.title,
                description: this.description,
                todoList: this.tasks,
                editMode: false
            })
            this.title = ''
            this.description = ''
            this.taskName = ''
            this.tasks = []
        },
        emitUpdateProj(){
            this.$emit('onUpdateProj', {
                id: this.proj.id,
                title: this.title,
                description: this.description,
                todoList: this.tasks,
                editMode: false
            })
        }

    }
}
</script>

<style>
.container{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    flex-wrap: wrap;
}
</style>