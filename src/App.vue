<script setup>
  import {onMounted, ref} from 'vue'
  
  const divinationArray = ref([])
  const discoveredArray = ref([])
  const divined = ref('')

  const diviner = () => {
    let random = randomFunc()

    discoveredArray.value[random] = divinationArray.value[random]

    divined.value = divinationArray.value[random]

    //  TO-DO: Handle duplicates
  }

  const loadDivinations = () => {
    fetch('/programmingProject3/assets/divinations.txt').then(getDivinations).then(processData)
  }

  const getDivinations = (divinationArray) => {
    return divinationArray.text()
  }

  const processData = (divinationsRaw) => {
    // Split by new line, use regex to handle \r or \n
    divinationArray.value = divinationsRaw.split(/\r?\n/);

    for (let i = 0; i < divinationArray.value.length; i++) {
      discoveredArray.value.push("<b>Not yet discovered...</b>");
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
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@400..700&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap" rel="stylesheet">

  <main class="app">
    <section class="heading">
      <h1>Programming Project 3</h1>

      <h2><i>Divine your fortune!</i></h2>
    </section>

    <section class="content">
      <div>
        <h2>What will you divine today?</h2>

        <button id="divineFortune" type="button" v-on:click="diviner()">Divine!</button>
        <p id="fortune" v-if="divined !== ''">{{ divined }}</p>
        <p id="fortune" v-else>You haven't divined anything yet!</p>
      </div>

      <div id="discovered">
        <h2>Discovered Fortunes</h2>
        <ul>
          <li v-for="discovered in discoveredArray" v-html="discovered"></li>
        </ul>
      </div>
    </section>
  </main>
</template>