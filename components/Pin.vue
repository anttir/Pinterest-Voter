<template>
    <div class="pin">
        <img :id="'pin_' + pin.id" :alt="pin.note" :src="pin.image.original.url" class="pinimage" />
        <label :for="'pin_' + pin.id" :title="pin.note">{{pin.note | shorten}}</label>
    </div>
</template>

<script>
export default {
  name: "Pin",
  props: ['pin']
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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