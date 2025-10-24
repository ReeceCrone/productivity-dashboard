<template>
  <div class="flex flex-col h-full bg-slate-800 text-white p-3 rounded-lg">
    <!-- Header with League Selector -->
    <div class="flex items-center justify-between mb-2">
      <h2 class="text-xl font-bold">🏆 {{ league }} Live Games</h2>
      <select
        v-model="league"
        @change="fetchScores"
        class="bg-slate-700 text-white rounded px-2 py-1 text-sm focus:outline-none"
      >
        <option>NBA</option>
        <option>NFL</option>
        <option>MLB</option>
        <option>NHL</option>
      </select>
    </div>

    <!-- No Live Games -->
    <div
      v-if="games.length === 0"
      class="text-slate-400 text-center flex-1 flex items-center justify-center"
    >
      No live games right now.
    </div>

    <!-- Live Games List -->
    <ul v-else class="space-y-2 overflow-y-auto">
      <li
        v-for="game in games"
        :key="game.id"
        class="bg-slate-700 rounded p-2 transition hover:bg-slate-600"
      >
        <div class="flex justify-between items-center">
          <span>{{ game.awayTeam }} @ {{ game.homeTeam }}</span>
          <span class="font-bold text-lg">
            {{ game.awayScore }} - {{ game.homeScore }}
          </span>
        </div>
        <div class="text-sm text-slate-400 mt-1">
           {{ game.status }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const league = ref('NBA')
const games = ref([])
const loading = ref(true)
let intervalId = null

async function fetchScores() {
  loading.value = true
  try {
    let url = ''
    switch (league.value) {
      case 'NBA':
        url = 'https://site.api.espn.com/apis/site/v2/sports/basketball/nba/scoreboard'
        break
      case 'NFL':
        url = 'https://site.api.espn.com/apis/site/v2/sports/football/nfl/scoreboard'
        break
      case 'MLB':
        url = 'https://site.api.espn.com/apis/site/v2/sports/baseball/mlb/scoreboard'
        break
      case 'NHL':
        url = 'https://site.api.espn.com/apis/site/v2/sports/hockey/nhl/scoreboard'
        break
    }

    const res = await fetch(url)
    const data = await res.json()

    // Filter only currently playing games
    const liveEvents = data.events.filter(
      (event) => event.competitions[0].status.type.state === 'in'
    )

    games.value = liveEvents.map((event) => {
      const comp = event.competitions[0]
      const home = comp.competitors.find((c) => c.homeAway === 'home')
      const away = comp.competitors.find((c) => c.homeAway === 'away')
      return {
        id: event.id,
        homeTeam: home.team.displayName,
        awayTeam: away.team.displayName,
        homeScore: home.score,
        awayScore: away.score,
        status: comp.status.type.shortDetail
      }
    })
  } catch (err) {
    console.error('Error fetching sports data:', err)
  } finally {
    loading.value = false
  }
}

// Watch league changes
watch(league, () => {
  clearInterval(intervalId)
  fetchScores()
  intervalId = setInterval(fetchScores, 30000)
})

onMounted(() => {
  fetchScores()
  intervalId = setInterval(fetchScores, 10000)
})
</script>

<style scoped>
ul::-webkit-scrollbar {
  width: 6px;
}
ul::-webkit-scrollbar-thumb {
  background-color: rgba(100, 116, 139, 0.5);
  border-radius: 10px;
}
ul::-webkit-scrollbar-thumb:hover {
  background-color: rgba(148, 163, 184, 0.8);
}
</style>

