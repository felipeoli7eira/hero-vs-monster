<script setup lang="ts">
// import TheWelcome from '../components/TheWelcome.vue'
import { ref, watch, computed } from "vue"

// * data
let heroLife    = ref(100)
let monsterLife = ref(100)
const gameLogs   = ref([])

let hasResult   = computed(() => {
  return heroLife.value === 0 || monsterLife.value === 0
})

let running     = ref(false)

// * watch
watch(hasResult,   (newValue, oldValue) => {
  if (newValue) {
    running.value = false
  }
})

// * methods

function reduceLife(character: string, howMuch: number)
{
  character === 'hero' ?
    heroLife.value = Math.max(heroLife.value - howMuch, 0)
  :
    monsterLife.value = Math.max(monsterLife.value - howMuch, 0)
  ;
}

function gameLog(text: string, className: string)
{
  gameLogs.value.unshift({ text, className })
}

function startGame(): void
{
  running.value     = true
  heroLife.value    = 100
  monsterLife.value = 100
  gameLogs.value = []
}

function classHeroLife() {
  return {
    'text-red-500': heroLife.value < 30
  }
}

function classMonsterLife() {
  return {
    'text-red-500': monsterLife.value < 30
  }
}

function attack(): void
{
  const hurt: number = randomNumber(5, 10)
  reduceLife('monster', hurt)
  gameLog(`Heroi atacou monstro com ${hurt} de força.`, 'from-hero')

  if (monsterLife.value > 0) {
    const hurt: number = randomNumber(7, 12)
    reduceLife('hero', hurt)
    gameLog(`Monstro atacou heroi com ${hurt} de força.`, 'from-monster')
  }
}

function specialAttack(): void
{
  const hurt: number = randomNumber(5, 10) + randomNumber(3, 6)
  reduceLife('monster', hurt)
  gameLog(`Heroi atacou monstro com super ataque de ${hurt} de força`, 'from-hero')

  let counterBlow: number = randomNumber(7, 12)
  reduceLife('hero', counterBlow)
  gameLog(`Monstro revidou o super ataque com ${counterBlow} de força`, 'from-monster')
}

function toGiveUp()
{
  running.value = false
  gameLog('Ah não! o heroi desistiu...', 'give-up')
}

function heal()
{
  if (heroLife.value < 100) {

    const heal: number = randomNumber(10, 15)
    heroLife.value = Math.min(heroLife.value + heal, 100)

    gameLog(`Heroi recebeu ${heal} de vida.`, 'hero-heal')

    // hurt
    // heroLife.value    = Math.max(heroLife.value - randomNumber(7, 12), 0)
  }
}

function randomNumber(min: number, max: number): number {
  return Math.round(
    Math.random() * (max - min) + min
  )
}

</script>

<template>
  <main id="game-view">
    <!-- <TheWelcome /> -->

    <div class="players">

      <div class="hero">
        <h1>Hero</h1>
        <p :class="classHeroLife()">{{ heroLife }}</p>
      </div>

      <div class="hero">
        <h1>Monster</h1>
        <p :class="classMonsterLife()">{{ monsterLife }}</p>
      </div>

    </div>

    <div v-if="!hasResult" class="controls">

      <template v-if="running">
        <button @click="attack" class="rounded-2xl bg-blue-600 p-2 font-semibold text-white hover:bg-blue-500 focus:outline-none focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-500 active:text-white/70">Atacar</button>
        <button @click="specialAttack" class="rounded-2xl bg-blue-600 p-2 font-semibold text-white hover:bg-blue-500 focus:outline-none focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-500 active:text-white/70">Super ataque</button>
        <button :disabled="heroLife === 100" @click="heal" class="rounded-2xl bg-blue-600 p-2 font-semibold text-white hover:bg-blue-500 focus:outline-none focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-500 active:text-white/70">Cura</button>
        <button @click="toGiveUp" class="rounded-2xl bg-blue-600 p-2 font-semibold text-white hover:bg-blue-500 focus:outline-none focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-500 active:text-white/70">Desistir</button>
      </template>

      <button v-else @click="startGame" class="rounded-2xl bg-blue-600 p-2 font-semibold text-white hover:bg-blue-500 focus:outline-none focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-500 active:text-white/70">Começar batalha</button>
    </div>

    <div v-if="hasResult" class="result">
      <div v-if="heroLife" class="win text-green-700">Vencedor! Você derrotou o montro.</div>
      <div v-if="!heroLife" class="loser text-red-700">Perdedor! O monstro derrotou você</div>
    </div>

    <div v-if="gameLogs.length" class="actions-log">
      <ul class="game-logs">
        <li class="log" v-for="(log, index) in gameLogs" :key="index">
          {{ log.text }}
        </li>
      </ul>
    </div>
  </main>
</template>
