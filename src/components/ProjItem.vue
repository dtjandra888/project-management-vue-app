<template>
<div>
    <div v-if="!editMode">
        <span class="item-element">
            <strong> {{ proj.title }}</strong>
        </span>
        
        <button @click="toggleEditProj" type="button" class="btn btn-outline-primary">Edit</button>
        <button @click="removeProj" type="button" class="btn btn-danger">X</button>
        
        <br/>
        <!--Project Description-->
        <span class="item-element">
            {{ proj.description }}
        </span>

        <!--Rendering the tasks for the project-->
        <div v-for="task in proj.todoList" :key="task.id">
            <div class="form-check">
                <input class="form-check-input" type="checkbox" value="" id="flexCheckDefault">
                <label class="form-check-label" for="flexCheckDefault">
                    {{ task.name }}
                </label>
            </div>
        </div>

    </div>

    <div v-else>
        <PJForm
            @onUpdateProj="handleUpdateProj"
            :proj="proj"
            isEditMode
        />
    </div>
</div>
</template>

<script>
import PJForm from './PJForm.vue'

export default{
    name: 'ProjectItem',
    components:{
        PJForm
    },
    props: {
        proj: {
            type: Object,
            required: true
        }
    },
    computed:{
        editMode(){
            return this.proj.editMode
        }
    },

    methods: {
        toggleEditProj(){
            this.$emit('onToggleEditProj', this.proj.id)
        },
        handleUpdateProj(updatedProj){
            this.$emit('onUpdateProj', updatedProj)
        },
        removeProj(){
            this.$emit('onRemoveProj', this.proj.id)
        }
    }
}
</script>

<style>
button{
    color: #fff;
}
.item-element{
    margin: 10px;
    font-size: 1.6em;
}
li{
    list-style: checkbox;
}
</style>