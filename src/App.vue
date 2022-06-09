<template>
<div class="page">
 <h1>Task Load on Scroll</h1>
 <button @click="scrolll">Scroll</button>
 <section v-for="(item) in list_of_data" v-bind:key="item.id" class="card show">
 <h2>{{item.title}}</h2>
 <p>{{item.body}}</p>
 </section>
</div>

</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data(){
    return{
      list_of_data:null,
    }
  },
  
  mounted () {
    axios
      .get('https://jsonplaceholder.typicode.com/posts?_page=1&_limit=10')
      .then(response => (this.list_of_data = response.data)).catch(err=>console.log(err));

  }
,
 updated(){
   const cards= document.querySelectorAll(".card");

      const observer = new IntersectionObserver(entries=>{
        entries.forEach(entry=>{
          entry.target.classList.toggle("show",entry.isIntersecting)
        })
      });

      cards.forEach(card=>{
        observer.observe(card);
        console.log("Card :",card);
      })
 }

  
}
</script>

<style>

*{
   margin: 0px;
}
h1{
  text-align: center;
  margin-top: 50px;
}
.page{
  height: 100vh;
 
  display: flex;
  flex-direction: column;
  align-items: center;

}
.card{
  border: 1px solid red;
  width: 30%;
  padding: 20px;
  margin-top: 20px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: 1500ms;
  opacity: 0;
}
.card.show{
  transform: translateX(0);
  opacity: 1;
}
</style>
