<template>
  <div class="home">
    <FilterNav @filterChange="current=$event"  :current="current"/>
    <div v-if="projects.length">

      <div v-for="project in filteredProjects" :key="project.id">
      
        <SinglePage  :project="project" @delete="handleDelete" @complete="handlecomplete"/>
      
      </div>

    </div>
  </div>
</template>

<script>
import SinglePage from '../components/SinglePage.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {
    SinglePage,
    FilterNav
  },

  data(){
    return {
      projects:[],
      current:'all'
    }
  },

  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },

  methods: {
    handleDelete(id){
      this.projects = this.projects.filter((project)=>{
        return project.id !==id
      })
    },
    handlecomplete(id){

      let p = this.projects.find(project =>{
        return project.id === id
      })

      p.complete = !p.complete
    }
  },

  computed:{

    filteredProjects(){

      if(this.current === 'completed')
      {
        return this.projects.filter(project => project.complete)
      }

      if(this.current === 'ongoing')
      {
        return this.projects.filter(project => !project.complete)
      }

      return this.projects
    }
  }

}
</script>
