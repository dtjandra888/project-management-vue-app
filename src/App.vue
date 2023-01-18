<template>
  <div id="app">
    <h1>Attempt at pjm app</h1>
    <PJForm
      @onNewProject="handleNewProject"
    />

    <ProjItem 
      v-for="proj in projectList" 
      :key="proj.id" 
      :proj="proj"
      @onToggleProj="handleToggleProj"
      @onToggleEditProj="handleToggleEditProj"
      @onUpdateProj="handleUpdateProj"
      @onRemoveProj="handleRemoveProj"
    />

  </div>
</template>

<script>
import PJForm from "./components/PJForm.vue";
import ProjItem from "./components/ProjItem.vue"

export default {
  name: 'App',
  components: {
    PJForm,
    ProjItem
  },

  data(){
    return {
      projectList: []
    }
  },

  methods: {
    handleNewProject(newProj){
      this.projectList.push(newProj)
    },

    handleToggleProj({value, id}){
      const project = this.findProject(id)
      project.isDone = value
    },

    handleToggleEditProj(id){
      const project = this.findProject(id)
      project.editMode = !project.editMode
    },

    handleUpdateProj(updatedProj){
      this.projectList = this.projectList.map((proj) => {

        if(proj.id === updatedProj.id) {
          return updatedProj
        }

        return proj
      })
    },
    handleRemoveProj(id){
      return this.projectList = this.projectList.filter((p) => p.id !== id)
    },
    findProject(id){
      return this.projectList.find(proj => proj.id === id)
    }
  }
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
PJForm{
  padding: 10px;
}
ProjItem{
  padding: 5px;
}
</style>
