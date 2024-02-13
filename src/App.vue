<template>
  <div class="container">
    <h1>Conditional Rendering & Event Handling</h1><hr/>
    <div>
      <h2>V-bind :</h2>
      <img :src="url" class="image"/>
      <h2 :style="{ backgroundColor: isImportant ? 'lightcoral' : 'lightgray' }">Random Text</h2>
      <h2 :class="{myClass:isImportant}">Hello Vue JS</h2>
    </div>
    <hr style="margin-top: 10px;"/>
    <div>
      <h2>V-if :</h2>
      <div v-if="text.includes('pizza')">
        <p>The text includes the word 'pizza'</p>
        <img class="image" src="https://img.freepik.com/free-photo/freshly-italian-pizza-with-mozzarella-cheese-slice-generative-ai_188544-12347.jpg">
      </div>
      <p v-else>The word 'pizza' is not found in the text</p>
    </div>
    <hr/>
    <div>
      <h2>V-for</h2>
      <ul class="row" v-for="i in foods">
        <li>{{ i.id }} : {{ i.name }}</li>
      </ul>
    </div>
    <hr/>
    <div>
      <h2>V-on</h2>
      <div>
        <button type="button" class="btn btn-primary" @click="count++">Increase</button>
        <button type="button" class="btn btn-primary ml-2" @click="count--">Decrease</button>
        <h3>Count : {{ count }}</h3>
        <form @submit.prevent="onSubmit">
          <label for="input">Enter name :</label>
          <input class="form-control" v-model="name" @keyup.enter="setName(event)" type="text" id="input">
          <h3>Name : {{ result }}</h3>
          <button type="button" class="btn btn-primary" @click="(event) => warn('Form cannot be submitted yet.', event)">
            Submit
          </button>
        </form>
      </div>
      <hr/>
      <div>
        <h2>Computed Property</h2>
        <label for="input">Enter Word :</label>
        <input class="form-control" v-model="oldWord" @keyup.enter="setWord(event)" type="text" id="input">
        <h3>You Entered : {{ newWord }}</h3>
        <h3>Reversed Senetnce : {{ modified }}</h3>
      </div>
    </div>
    <hr/>
    <div>
      <h2>Watch Property</h2>
      <label for="question">Enter Question :</label>
      <input class="form-control" v-model="question" type="text" id="question">
      <h3>Answer : {{ answer }}</h3>
      <img :src="responseData.image">
    </div>
  </div>
</template>

<script setup>
import {ref,reactive,computed,watch} from "vue"
import 'bootstrap'; 
import 'bootstrap/dist/css/bootstrap.min.css';

const url="https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Taj_Mahal%2C_Agra%2C_India.jpg/640px-Taj_Mahal%2C_Agra%2C_India.jpg"
const isImportant=true
const text= 'I like taco, pizza, Thai beef salad, pho soup and tagine.'
const foods=reactive([{id:1,name:"Apple"},{id:2,name:"Mango"},{id:3,name:"Guava"},{id:4,name:"Banana"}])
const count=ref(0)
const name=ref("")
const result=ref("")
const oldWord=ref("")
const newWord=ref("")
const question=ref("")
const answer=ref("")
const responseData=ref("")

const setName=(event)=>{
  result.value=name.value
}

const setWord=(event)=>{
  newWord.value=oldWord.value
}

const warn=(message,event)=>{
  alert(message)
}

const modified = computed(() => {
  return newWord.value.split('').reverse().join('');
});

watch(question,async(oldquestion,newquestion)=>{
  if(newquestion.indexOf('?') > -1){
    answer.value="Thinking"

    try {
      const res=await fetch("https://yesno.wtf/api");
      const resJson=await res.json();
      responseData.value=resJson
      console.log(responseData)
      answer.value=resJson.answer
    } catch (error) {
      answer.value="Error! Can not fetch from API."
    }
  }
})
</script>

<style>
.image{
  width: 200px;
  height: 160px;
}
.container{
  display: flex;
  flex-direction: column;
}
.myClass{
  background-color: burlywood;
}
</style>