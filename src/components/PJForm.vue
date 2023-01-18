<template>
<form @submit.prevent="submit">
    <input required placeholder="Title" v-model="title">
    <input required placeholder="Description" v-model="description">
    <button type="submit" class="btn btn-success">{{ isEditMode ? 'Save' : 'Add' }}</button>
</form>
</template>

<script>
let id = 0;
export default{
    name: "ProjectForm",
    data(){
        return {
            title: '',
            description: ''
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
        emitNewProject() {
            this.$emit('onNewProject', {
                id: id++,
                title: this.title,
                description: this.description,
                todoList: [],
                isDone: false,
                editMode: false
            })
            this.title = ''
            this.description = ''
        },
        emitUpdateProj(){
            this.$emit('onUpdateProj', {
                id: this.proj.id,
                title: this.title,
                description: this.description,
                todoList: [],
                isDone: this.proj.isDone,
                editMode: false
            })
        }

    }
}
</script>

<style>

</style>