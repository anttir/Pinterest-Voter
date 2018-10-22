<template>
  <section class="container">
    <h1>{{ msg }}</h1>
    <h2 @click="loadData">Essential Links</h2>
    <select v-model="boardUrl">
      <option v-for="board in boards" v-bind:value="board">{{ board }}</option>
    </select>
    <pin id="leftPin" :pin="currentBattle.leftPin"></pin>
    <div id="rightPin">

    </div> -->
    <ul class="pins">
      <li class="pin" v-for="pin in pins">
        <div>
          <img :id="'pin_' + pin.id" :alt="pin.note" :src="pin.image.original.url" class="pinimage" />
          <label :for="'pin_' + pin.id" :title="pin.note">{{pin.note | shorten}}</label>
        </div>
      </li>
    </ul>
  </section>
</template>

<script>
import Pin from '~/components/Pin.vue'
const baseURL = 'https://api.pinterest.com/v1/boards/'
const baseAttributes =
  'pins/?access_token=Aqcz_spf3RhZtDPlRK92HyIp8LPrFV7N46frza1FUqm0hcBwrwJUQDAAABEFRVKuoFdAUXEAAAAA&limit=20&fields=id,image,note'

export default {
  name: 'PinterestVote',
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
.pin {
  margin: 0 10px;
  width: 15vw;
  min-width: 100px;
}
.pin label {
}
.pinimage {
  display: block;
  height: 15vw;
  width: 15vw;
  min-height: 100px;
  min-width: 100px;
  /* object-fit: cover; */
  /* width: 50px;
  height: 100px; */
}
</style>
