<template>
<div class="project" :class="{complete:project.complete}">

   <div >
        <div class="flexing">
                <div @click="showDetail=!showDetail">
                    <h2>{{project.title}}</h2>
                </div>
                <div>
                <span class="material-icons" @click="deleteProject">
                delete
                </span>
                <router-link :to="{name:'EditProject',params:{id:project.id}}" >
                   <span class="material-icons">
                edit
                </span>
                 </router-link>
             
                <span class="material-icons" @click="completeProject">
                done
                </span>
                </div>
        </div>
    </div>
    <p v-if="showDetail">{{project.detail}}</p>
    {{project.complete}}
</div>
</template>

<script>
export default {

props:['project'],
data() {
    return {
        showDetail:false,
        api:"http://localhost:3000/projects/"
        
    }
},
methods: {
    deleteProject(){
        let deleteRoute=this.api+this.project.id;
          fetch(deleteRoute,{method:"DELETE"})
            .then(() =>{
                this.$emit("delete",this.project.id)
            })
            .catch((err) => {
                console.log(err);
            })
    },
    completeProject(){
        let updateCompleteRoute =this.api+this.project.id;
        fetch(updateCompleteRoute,{
            method:"PATCH",
            headers:{
                "Content-Type":"application/json"
            },
                body:JSON.stringify({
                    complete:!this.project.complete
                })
            })
            .then(() => {
                    this.$emit("complete",this.project.id)
            })
            .catch(() => {

            })
      
    }
 
},
}
</script>

<style>
.project{
    padding:20px;
    background-color: #f2f2f2;
    border-left:6px solid crimson;
    margin: 10px;
    border-radius: 8px;
}
.h2{
    color: indigo;
    cursor: pointer;
}
.flexing{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
span{
    margin-left: 6px;
}
span:hover{
    cursor: pointer;
    color: #777;
}
.complete{
    border-left:6px solid green;
}
</style>