<template>
  <div id="app">
    <nav class="navbar navbar-expand-sm bg-light">
      <div class="container-fluid">
        <span class="navbar-brand mb-0 h1">PJM-App</span>
      </div>
    </nav>

    <div class="grid-container">
      <div class="form-container" >
        <h2 style="margin-top: 10px;">Add new Project</h2>
        <PJForm
          @onNewProject="handleNewProject"
        />
      </div>

      <div class="project-container">
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
    </div>
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
      console.log(newProj)
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
}
.grid-container {
  display: grid;
  grid-template-columns: 0.5fr 1fr;
}
.form-container{
  margin-top: 15px;
}
.project-container{
  margin-top: 15px;
  text-align: left;
}

</style>
