<template>
<div class="page">
 <div v-for="(item,index) in list_of_data" v-bind:key="index" class="card">
 <h2>{{item.title}}</h2>
 <p>{{item.body}}</p>
 </div>
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
  
  beforeMount () {
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
          if(entry.isIntersecting){
            //load just once
            observer.unobserve(entry.target)
          }
        })
      },{
        //load if 100% od section is intersect
        threshold:1
      });



      
      const cardContainer = document.querySelector('.page')

      const loadNewCards =()=>{
        for(let i=0;i<10;i++){
          const card =document.createElement('div');
          const header= document.createElement("h2")
          const parag= document.createElement("p")
          const contentHeader = document.createTextNode("HI I'M HEADER")
          const contentprg = document.createTextNode("HI I'M paragarpgh")
          header.appendChild(contentHeader)
          parag.appendChild(contentprg)
          card.appendChild(header);
          card.appendChild(parag);
          card.classList.add("card")
          observer.observe(card)
         cardContainer.append(card)
        }
      }

      
      const lastObserver = new IntersectionObserver ( entries=>{
        const lastcard =entries[0];
        if(!lastcard.isIntersecting) 
        {
          return
          }
          
        loadNewCards();
        lastObserver.unobserve(lastcard.target);
        
      },{
        threshold:1
      })

     



      cards.forEach(card=>{
        observer.observe(card);
        lastObserver.observe(card)
    
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
  transition: 0.5s ease-in;
  opacity: 0;
}
.card.show{
  transform: translateX(0.5);
  transition: 0.5s ease-in;
  opacity: 1;
}
</style>
