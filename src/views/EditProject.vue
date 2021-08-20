<template>
  <h1>Edit Project</h1>
   <form @submit.prevent="addProject">
    <label>Project Title</label>
    <input type="text" v-model="title">
    <label>Project Detail</label>
    <input type="text" v-model="detail">
    <button  @click="updateProject">Update Project</button>
 </form>

</template>

<script>
export default {
props:["id"],
data() {
    return {
        title:"",
        detail:""
    }
},
mounted() {
    fetch('http://localhost:3000/projects/'+this.id)
    .then((response) => {
        return response.json()
    })
    .then((datas) =>{
        this.title=datas.title,
        this.detail=datas.detail
    })
    .catch(() => {

    })
},
methods: {
    updateProject(){
        fetch('http://localhost:3000/projects/'+this.id,{method:"PATCH",
        headers:{
            "Content-type":"application/json"
        },
        body:JSON.stringify({
            title:this.title,
            detail:this.detail
        })
        })
        .then(() =>{
            this.$router.push({name:"Home"})
        })
        .catch((err) => {
            console.log(this.err);
        })
    }
},
}
</script>

<style>

</style>