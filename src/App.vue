<script setup>
  import {onMounted, ref} from 'vue'
  
  const divinationArray = ref([])
  const discoveredArray = ref([])
  const divined = ref('')

  const diviner = () => {
    let random = randomFunc()

    discoveredArray.value[random] = divinationArray.value[random]

    divined.value = divinationArray.value[random]
  }

  const loadDivinations = () => {
    fetch('src/assets/divinations.txt').then(getDivinations).then(processData)
  }

  const getDivinations = (divinationArray) => {
    return divinationArray.text()
  }

  const processData = (divinationsRaw) => {
    // Split by new line, use regex to handle \r or \n
    divinationArray.value = divinationsRaw.split(/\r?\n/);

    for (let i = 0; i < divinationArray.value.length; i++) {
      discoveredArray.value.push("Not yet discovered...");
    }
  }

  const randomFunc = () => {
    let num = Math.floor(Math.random() * divinationArray.value.length)

    return num; 
  }

  onMounted( () => {
    loadDivinations()
  })
</script>

<template>
  <main class="app">
    <section class="heading">
      <h1>Programming Project 2</h1>

      <h2>Divine your fortune!</h2>
    </section>

    <section class="content">
      <h3>What will you divine today?</h3>

      <button id="divineFortune" type="button" v-on:click="diviner()">Divine!</button>
      <p id="fortune">{{ divined }}</p>

      <div>
        <h2>Discovered Fortunes</h2>
        <ul>
          <li v-for="fortunes in discoveredArray">{{ fortunes }}</li>
        </ul>
      </div>
    </section>
  </main>
</template>