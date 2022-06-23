<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button below</p>
    <Option v-for="option in optionsArray" :key="option.title" :option="option" :options="options"/>
    <button class="primary" @click="computeSelectedNames">Find Names</button>
    <div class="cards-container">
      <CardName v-for="(name, index) in selectedNames" :key="name" :name="name" @remove="() => removeName(index)" :index="index"/>
    </div>
  {{ selectedNames }}
  </div>
</template>

<style scoped>

.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

h1 {
  font-size: 3rem;
}

.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}

.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}

</style>

<script setup lang="ts">

  import { Gender, Popularity, Length, names } from '@/data'
  
  interface OptionsState {
    gender: Gender;
    popularity: Popularity;
    length: Length;
  }

  const options = reactive<OptionsState>({
    gender: Gender.GIRL,
    popularity: Popularity.UNIQUE,
    length: Length.SHORT
  })

  const selectedNames = ref<string[]>([])

  const removeName = (index: number) => {
    const filteredNames = [...selectedNames.value]
    filteredNames.splice(index, 1)
    selectedNames.value = filteredNames
  }

  const computeSelectedNames = () => {
    const filteredNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if(options.length === Length.ALL) return true
      else return name.length === options.length
    })
    selectedNames.value = filteredNames.map(name => name.name)
  }

  const optionsArray = [
    {
      title: "1) Choose a gender",
      category: "gender",
      buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY]
    },
    {
      title: "2) Choose the name's popularity",
      category: "popularity",
      buttons: [Popularity.TRENDY, Popularity.UNIQUE]
    },
    {
      title: "1) Choose the name's length",
      category: "length",
      buttons: [Length.SHORT, Length.LONG, Length.ALL]
    }
  ]

</script>
