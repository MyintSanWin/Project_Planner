<template>
 <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="current=$event" :current="current"/>
    <div v-for="project in filteredProjects" :key="project.id" >
       <SingleProject :project="project" @delete="deleteProject" @complete="completeProject" ></SingleProject>
        <AddProject/>
    </div>
 </div>
 {{current}}
</template>

<script>
// @ is an alias to /src
import SingleProject from "../components/SingleProject.vue"
import addProject from "../views/AddProject.vue"
import FilterNav from "../components/FilterNav.vue"

export default {
  name: 'Home',
  components: {SingleProject,FilterNav},
  

  data(){
    return {
      projects:[],
      current:"all"
    }
 
  },
     methods: {
      deleteProject(id){

        this.projects=this.projects.filter(project=>{
          return project.id!=id;
        })
      },
      completeProject(id){
        let findProject = this.projects.find(project=>{
          return project.id===id;
        });
        findProject.complete=!findProject.complete;
      }
    },

    computed:{
        filteredProjects(){
          if(this.current==="complete"){
            return this.projects.filter((p)=>{
                return p.complete
            })
          }
          if(this.current==="ongoing"){
            return this.projects.filter((p)=>{
              return !p.complete
            })
          }
          return this.projects;
        }
    },

  mounted() {
    fetch('http://localhost:3000/projects')
    .then((response) => {
     return response.json();
    })
    .then((datas) => {
      this.projects=datas;
    })
    .catch(() => {

    })
  
  }

}
</script>
