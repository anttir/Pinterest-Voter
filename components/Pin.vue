<template>
    <div class="pin" v-if="pin.image" :class="size">
        <img :id="'pin_' + pin.id" :alt="pin.note" class="pinimage" :src="pin.image.original.url" />
        <label :for="'pin_' + pin.id" :title="pin.note">{{pin.note | shorten}}</label>
    </div>
</template>

<script>
export default {
  name: 'Pin',
  props: ['pin', 'size'],
  filters: {
    shorten(text) {
      return text.substring(0, 50)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.pin.large {
  width: 30vw;
}
.pin {
  margin: 0 10px;
  width: 15vw;
  min-width: 100px;
  display: inline-block;
}
.pin label {
}
.large .pinimage {
  height: 30vw;
  width: 30vw;
}
.pinimage {
  display: block;
  height: 15vw;
  width: 15vw;
  min-height: 100px;
  min-width: 100px;
  object-fit: contain; 
}
</style>