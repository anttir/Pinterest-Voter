<template>
  <section class="container">
    <h1>{{ msg }}</h1>
    <div>
      <select v-model="boardUrl">
        <option v-for="board in boards" v-bind:value="board" :key="board.id">{{ board }}</option>
      </select>
    </div>
    <voter :leftPin="currentBattle.leftPin" :rightPin="currentBattle.rightPin" ></voter>
    <ul class="pins">
      <li class="pin" v-for="pin in pins" :key="pin.id">
        <pin :pin="pin"></pin>
      </li>
    </ul>
  </section>
</template>

<script>
import Voter from '~/components/voter'
import Pin from '~/components/pin'
const baseURL = 'https://api.pinterest.com/v1/boards/'
const baseAttributes =
  'pins/?access_token=Aqcz_spf3RhZtDPlRK92HyIp8LPrFV7N46frza1FUqm0hcBwrwJUQDAAABEFRVKuoFdAUXEAAAAA&limit=20&fields=id,image,note'

export default {
  name: 'PinterestVote',
  components: { Pin, Voter },
  data() {
    return {
      msg: 'Hello',
      boards: [
        'saanakaihu/bags/',
        'saanakaihu/bathroom/',
        'saanakaihu/cottage/'
      ],
      boardUrl: 'saanakaihu/bags/',
      pins: [],
      error: '',
      results: [],
      currentBattle: { leftPin: {}, rightPin: {}, vote: null }
    }
  },
  mounted() {
    // when the Vue app is booted up, this is run automatically.
    this.loadData()
    if (localStorage.results) {
      this.results = localStorage.results
    }
  },
  methods: {
    loadData: function() {
      if (this.boardUrl) {
        let vm = this
        fetch(baseURL + this.boardUrl + baseAttributes)
          .then(resp => resp.json()) // Transform the data into json
          .then(function(data) {
            vm.pins = data.data
            vm.nextBattle()
          })
      }
    },
    nextBattle() {
      this.currentBattle.leftPin = this.pins[
        Math.floor(Math.random() * this.pins.length)
      ]
      this.currentBattle.rightPin = this.pins[
        Math.floor(Math.random() * this.pins.length)
      ]
      if (this.currentBattle.leftPin.id == this.currentBattle.rightPin.id)
        nextBattle()
      this.currentBattle.vote = null
    },
    savevote(leftPin, rightPin, vote) {
      var name = boardUrl.replace('/', '__')
      if (!results[name]) results[name] = { votes: [] }
      results[name].votes.push({
        leftPinId: leftPin.id,
        pinRightId: rightPin.id,
        vote: vote
      })
      localStorage.results[name] = results[name]
      nextBattle()
    }
  },
  filters: {
    shorten(text) {
      return text.substring(0, 50)
    }
  },
  watch: {
    // whenever question changes, this function will run
    boardUrl: function() {
      this.loadData()
    }
  }
}
</script>

<style>
.container {
  width: 100vw;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
}
h1,
h2 {
  font-weight: normal;
}
.pins {
  list-style-type: none;
  display: flex;
  flex-wrap: wrap;
  padding: 0;
}
a {
  color: #42b983;
}
</style>
