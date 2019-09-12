<template>
<!-- Форма "Стороны и объекты договора" не использует Vue -->
  <form class="form" action="javascript: void(0)">
    <!-- Участники договора -->
    <div class="container__form-input" id="vueContainer__form-input">
      <div class="">Участники договора</div>
        <button class="button button__plus" id="vueAddNewInput" type="submit" @click="pushInput">+</button>
        <div class="container__input" v-for="input in inputs">
          <input class="nameField" type="text" placeholder="" name='name' v-bind:value=input.name >
          <input class="phoneField" type="tel" placeholder="+7 000 000 000" name='phone' v-bind:value=input.phone>
        </div>
        
        

  </div>
    <!-- Вид собственности -->
    <div class="container__form-input">
      <div class="">Вид собственности</div>
      <label for="typeField"></label>
      <select class="typeField" id="typeField" v-on:change='changeSelect'>
        <option value="1">Общая собственность</option>
        <option value="2">Частная собственность</option>
        <option value="3">Государственная собственность</option>
      </select>
    </div>
    <div class="container__form-input">
      <ul class="type-container" id="type-container" >
        <li class="vid__item" v-for="item in json.main">{{item.title}} <span class="vid__item-date">{{item.value}}</span></li>
      </ul>
    <div id="totalArea">
      <div  class="vid__item">Площадь.......<span id="totalArea" class="vid__item-date">{{ json.totalArea }}</span></div>
    </div>
    </div>
    <div class="tabel" id="tabel">
      <div class="container-tabel" v-for="item in json.area">
       <div class="">{{item.title}}</div>
       <ul>
         
           <li class="tabel__item" v-for="itemArea in item.detail">{{itemArea.title}}<span class="tabel__date">{{itemArea.value}}</span></li>
       </ul>
    </div>
    </div>
    
    <div class="send-button-container">
      <div class="send-button"><button class="button" id="send" type="submit" @click="sendButton">Send</button></div>
      <div class=""><button class="button" id="load" type="button" @click="load" >Load</button></div>
    </div>
  </form>
  <!-- END! Форма "Стороны и объекты договора" использует Vue -->
</template>

<script>
import vueDataJson from '../../date.json';

const names = ['Иванов', 'Смирнов Владимир', 'Кузнецов Алексей', 'Попов Игорь', 'Петров Петр', 'Новикова Алена', 'Волков Сергей', 'Егоров Егор', 'Федор Бондарчук', 'А я Никита']
const inputs = [
    {
      name: '',
      phone: ''
    }
  ]

export default {
  data () {
    return {
      inputs: inputs,
      names: names,
      data: vueDataJson,
      json: vueDataJson[1]
    }
  },
  methods: {
    pushInput() {
      this.inputs.push({name:'', phone:''})
    },
    load() {
      const nameFields = document.querySelectorAll('.nameField')
      const phoneFields = document.querySelectorAll('.phoneField')

      this.inputs

      nameFields.forEach((item, i) => {
      if (item.value) return
      
      //генерируем Имя и Фамилию из массива
      this.inputs[i].name = this.names[Math.floor(Math.random() * (10))]
      //item.value = this.names[Math.floor(Math.random() * (10))]
     })

      phoneFields.forEach((item, i) => {
      if (item.value) return
      //генерируем номер телефона в формате +7 (9хх) хххххх
      this.inputs[i].phone = `+7 (9${Math.floor(Math.random()* 100)}) ${Math.floor(Math.random()* 1000000)}`
      //item.value = `+7 (9${Math.floor(Math.random()* 100)}) ${Math.floor(Math.random()* 1000000)}`
      })
    },
    sendButton() {
      const dataForSend = this.json
      const inputsContainer =  document.querySelectorAll(".container__input")
      for (let i = 0; i < inputsContainer.length; i+= 1) {
        let inputs = inputsContainer[i].children
        let personNumber = `person${i + 1}`
        dataForSend[personNumber] = {}
        for (let i = 0; i < inputs.length; i+= 1) {
          let name = inputs[i].name
          dataForSend[personNumber][name] = inputs[i].value
        }
      }

      fetch('127.0.0.1', {method: 'POST', body:JSON.stringify(dataForSend)})
      .then(() => alert(dataForSend))
      .catch(error => alert(error))
    },
    changeSelect(e) {
      console.log(e.target.value)
      this.json = this.data[e.target.value]
    }
  },
  created () {
    // this.json = vueDataJson[0]
  }
}
</script>

<style lang="scss">
</style>
