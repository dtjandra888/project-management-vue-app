<template>
<div>
    <div v-if="!editMode">
        <input type="checkbox" v-model="isDone">
        <span>
            <strong> {{ proj.title }}</strong>
        </span>
        <span>
            {{ proj.description }}
        </span>
        <button @click="toggleEditProj" type="button" class="btn btn-outline-primary">Edit</button>
        <button @click="removeProj" type="button" class="btn btn-danger">X</button>
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
        isDone: {
            get(){
                return this.proj.isDone
            },
            set(value){
                this.$emit('onToggleProj', {
                    value,
                    id: this.proj.id
                })
            }
        },
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
span{
    margin: 0 20px;
}
button{
    color: #fff;
}
</style>