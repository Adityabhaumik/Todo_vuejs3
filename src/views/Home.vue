<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handelDelete" @complete="handelComplete" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
 import SingleProject from '../components/SingleProject'
 import FilterNav from '../components/FilterNav'

export default {
  name: 'Home',
  data(){
    return{
      projects:[],
      current:'all'
    }
  },
  components: { SingleProject , FilterNav },
  mounted() {
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data =>this.projects = data)
    .catch(e=>{console.log(e.message)})
  },
  methods:{
      handelDelete(id){
      this.projects = this.projects.filter((project)=>{
        return project.id !== id
      })
    },
     handelComplete(id){
      let p = this.projects.find(project=>{
        return project.id == id
      })
      p.complete=!p.complete
    }
  },

  computed:{
    filteredProjects(){
      if(this.current ==='completed'){
        return this.projects.filter(project => project.complete)
      }
      else if(this.current === 'ongoing'){
         return this.projects.filter(project => !project.complete)
      }
    
        return this.projects
      
    }
  }
}
</script>
