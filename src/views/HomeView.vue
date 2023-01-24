<template>
  <div class="home">
    <Filternav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
          <SingleProject  @delete="handleDelete" @complete="handleComplete" 
          :project="project" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from "../components/SingleProject.vue"
import Filternav from "../components/Filternav.vue"

export default {
  name: 'HomeView',
  components: { SingleProject,Filternav },
  data(){
    return {
      projects:[],
      current: '',
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then( data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplete(id){
      let p = this.projects.find(projec => {
        return projec.id === id
      })
      p.complete = !p.complete
    }
  },
  computed:{
    filteredProjects(){
      if (this.current ==='completed'){
        return this.projects.filter((project)=> project.complete)
      }
      if(this.current ==='ongoing'){
        return this.projects.filter(project=> !project.complete)}
      return this.projects

    }
  }

}
</script>
