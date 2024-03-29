<script setup>
  import { ref, computed, onMounted, nextTick } from 'vue'

  const wins = ref(0)
  const draws = ref(0)
  const losses = ref(0)

  const choice = ref(null)
  const computerChoice = ref(null)
  const verdict = ref(null)
  const verdictClass = ref(null)

  const outcomes = {
    rock: {
      rock: 'draw',
      paper: 'loss',
      scissors: 'win'
    },
    paper: {
      rock: 'win',
      paper: 'draw',
      scissors: 'loss'
    },
    scissors: {
      rock: 'loss',
      paper: 'win',
      scissors: 'draw'
    }
  }

  const winPercentage = computed(() => {
    const total = wins.value + draws.value + losses.value
    return total ? (wins.value / total) * 100 : 0
  })

  const Play = (c) => {
    choice.value = c

    const choices = ['rock', 'paper', 'scissors']
    const random = Math.floor(Math.random() * choices.length)
    computerChoice.value = choices[random]

    const outcome = outcomes[c][computerChoice.value]

    console.log(outcome)

    if (outcome === 'win') {
      wins.value++
      verdict.value = 'You Won!'
      verdictClass.value = 'text-green-500'
    } else if (outcome === 'loss') {
      console.log('losses', losses.value)
      losses.value++
      verdict.value = 'You Lost!'
      verdictClass.value = 'text-red-500'
    } else {
      draws.value++
      verdict.value = 'It is a Draw!'
      verdictClass.value = 'text-yellow-500'
    }

    SaveGame()
  }

  const SaveGame = () => {
    localStorage.setItem('wins', wins.value)
    localStorage.setItem('draws', draws.value)
    localStorage.setItem('losses', losses.value)
  }

  const LoadGame = () => {
    wins.value = parseInt(localStorage.getItem('wins')) || 0
    draws.value = parseInt(localStorage.getItem('draws')) || 0
    losses.value = parseInt(localStorage.getItem('losses')) || 0
  }

  const ResetRound = () => {
    choice.value = null
    computerChoice.value = null
    verdict.value = null
  }

  onMounted(() => {
    LoadGame()
    window.addEventListener('keypress', (e) => {
      if (e.key === 'r') {
        ResetRound()
      }
    })
  })
</script>

<template>
  <div class="min-h-screen bg-cyan-700">
    <div class="mx-auto flex max-w-3xl flex-col text-center text-white">
      <header class="container mx-auto mt-20 p-3 sm:p-6">
        <h1 class="text-4xl font-bold">Rock, Paper, Scissors</h1>
      </header>

      <main class="container mx-auto flex-1 p-6">
        <div
          v-if="choice === null"
          class="mx-6 flex items-center justify-center"
        >
          <button
            @click="Play('rock')"
            class="mx-6 w-64 rounded-full bg-white p-6 shadow-lg transition-transform duration-300 hover:scale-110 sm:p-12"
          >
            <img
              src="./assets/RockIcon.svg"
              alt="Rock"
              class="h-10 w-10 max-w-10 sm:h-auto sm:w-full sm:max-w-full"
            />
          </button>
          <button
            @click="Play('paper')"
            class="mx-6 w-64 rounded-full bg-white p-6 shadow-lg transition-transform duration-300 hover:scale-110 sm:p-12"
          >
            <img
              src="./assets/PaperIcon.svg"
              alt="Rock"
              class="h-10 w-10 max-w-10 sm:h-auto sm:w-full sm:max-w-full"
            />
          </button>
          <button
            @click="Play('scissors')"
            class="mx-6 w-64 rounded-full bg-white p-6 shadow-lg transition-transform duration-300 hover:scale-110 sm:p-12"
          >
            <img
              src="./assets/ScissorsIcon.svg"
              alt="Rock"
              class="h-10 w-10 max-w-10 sm:h-auto sm:w-full sm:max-w-full"
            />
          </button>
        </div>

        <div v-else>
          <div class="mb-6 flex justify-evenly">
            <div class="text-3xl">
              You <br />
              <span class="font-bold uppercase">{{ choice }}</span>
            </div>
            <div class="text-3xl">
              Computer <br />
              <span class="font-bold uppercase">{{ computerChoice }}</span>
            </div>
          </div>
          <div
            class="mb-6 text-6xl"
            :class="verdictClass"
          >
            {{ verdict }}
          </div>
          <button
            @click="ResetRound"
            class="rounded-lg bg-white px-4 py-2 text-lg uppercase text-cyan-700"
          >
            Reset
          </button>
        </div>

        <div class="mb-4 mt-12 text-3xl">{{ wins }} | {{ draws }} | {{ losses }}</div>

        <div class="text-lg">Win rate: {{ Math.round(winPercentage) }}%</div>
      </main>
    </div>
  </div>
</template>

<style scoped></style>
