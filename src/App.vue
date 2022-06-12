<template>
<div class="page">
 <section v-for="(item,index) in list_of_data" v-bind:key="index" class="card">
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
   const lastC = document.querySelectorAll(".card:last-child")

      const observer = new IntersectionObserver(entries=>{
        entries.forEach(entry=>{
          entry.target.classList.toggle("show",entry.isIntersecting)
          console.log("lc",lastC)
          if(entry.isIntersecting){
            //load just once
            observer.unobserve(entry.target)
          }
        })
      },{
        //load if 100% od section is intersect
        threshold:1
      });

      
      const lastObserver = new IntersectionObserver ( entries=>{
        const lastcard =entries[0];
        if(!lastcard.isIntersecting) 
        {
          console.log("LAst card intersect")
          return
          }
          console.log("lastCArd Not Intersect")
        loadNewCards();
        lastObserver.unobserve(lastcard.target);
        
      },{
        root:null,
        threshold:0
      })

      //lastObserver.observe(lastC)


      const cardContainer = document.querySelector('.page')

      const loadNewCards =()=>{
        for(let i=0;i<10;i++){
          const card =document.createElement('section');
          const header= document.createElement("h2")
          const parag= document.createElement("p")
          const contentHeader = document.createTextNode("HI I'M HEADER")
          const contentprg = document.createTextNode("HI I'M paragarpgh")
          header.appendChild(contentHeader)
          parag.appendChild(contentprg)
          card.appendChild(header,parag);
          console.log("NEWCARD",card)
          card.classList.add("card")
          observer.observe(card)
         cardContainer.appendChild(card)
        }
      }

      cards.forEach(card=>{
        observer.observe(card);
    
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
  transition: 800ms;
  opacity: 0;
}
.card.show{
  transform: translateX(0);
  opacity: 1;
}
</style>
