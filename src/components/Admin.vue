<script setup>
import { ref } from 'vue'

const allPeople = ['Ariadine','Lars','Zishan','Marijn','Albert','Connor','Ismail','Nikola']
const teams = ref([])

const isLastFiveMinutes = ref(false)
const countdownDisplay = ref('')
const intervalId = ref('')

const shuffleArray = (array) => {
  let currentIndex = array.length,  randomIndex

  while (currentIndex != 0) {
    randomIndex = Math.floor(Math.random() * currentIndex)
    currentIndex--
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex], array[currentIndex]]
  }

  return array;
}

const generateRandomTeams = () => {
    teams.value = []

    const copiedPeople = [...allPeople]
    const halfWayPoint = Math.floor(copiedPeople.length / 2)
    const shuffledPeople = shuffleArray(copiedPeople)
   
    teams.value.push(shuffledPeople.slice(0, halfWayPoint))
    teams.value.push(shuffledPeople.slice(halfWayPoint))
}

function startTimer(duration) {
    let timer = duration, minutes, seconds;
    intervalId.value = setInterval(function () {
        minutes = parseInt(timer / 60, 10);
        seconds = parseInt(timer % 60, 10);

        minutes = minutes < 10 ? "0" + minutes : minutes;
        seconds = seconds < 10 ? "0" + seconds : seconds;

        countdownDisplay.value = minutes + ":" + seconds;

        if (--timer < 0) {
            timer = 0;
        }

        if (timer < 300) {
            isLastFiveMinutes.value = true;
        }
    }, 1000);
}

function resetTimer() {
    clearInterval(intervalId.value)
    intervalId.value = null
    countdownDisplay.value = ''
    isLastFiveMinutes.value = false
}
</script>

<template>
  <div class="flex gap-10 m-8">
    <div class="w-1/3 flex flex-col gap-10">
        <div class="min-h-[300px] h-full rounded-xl flex flex-col pt-4 pb-4 bg-neutral-500">
            <p class="text-lg">Countdown</p>
            <div class="flex-1 items-center justify-center flex">
                <p :class="['text-9xl', {'text-red-700': isLastFiveMinutes}]">{{ countdownDisplay || '30:00' }}</p>
            </div>
            <div>
                <button v-if="!countdownDisplay" class="border-gray-50" type="button" @click="startTimer(1799)">Start</button>
                <button  v-else class="border-gray-50" type="button" @click="resetTimer">Reset</button>
            </div>
        </div>

        <div class="bg-neutral-500 p-4 h-full rounded-xl min-h-[300px] flex flex-col">
            <p class="text-lg">Teams</p>

            <div v-if="!teams.length" class="flex flex-1 items-center justify-center">
                <div class="flex flex-wrap justify-center gap-3">
                    <span v-for="person in allPeople" class="text-2xl">
                        {{ person }}
                    </span>
                </div>
            </div>

            <div v-if="teams.length" class="flex justify-around flex-1 items-center">
                <div v-for="team, i in teams" class="">
                    <p class="text-3xl font-semibold pb-4 underline">Team {{ i+1 }}</p>
                    <p v-for="person in team" class="text-2xl">{{ person }}</p>
                </div>
            </div>

            <div>
                <button class="border-gray-50" type="button" @click="generateRandomTeams">{{teams.length ? 'Re-s' : 'S'}}huffle</button>
            </div>
        </div>
    </div>

    <div class="bg-neutral-500 p-4 rounded-xl w-2/3 justify-center flex">
        <img src="../assets/nuxt.png" alt="nuxt" class="image-height" />
    </div>
  </div>
</template>

<style scoped>
.image-height {
    height: calc(100vh - 100px);
}
</style>