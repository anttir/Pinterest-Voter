<template>
    <div>
        <div v-if="leftPin && rightPin">
            <pin id="leftPin" :pin="leftPin" :size="'large'"></pin>
            <pin id="rightPin" :pin="rightPin" :size="'large'"></pin>
            <div>
                1 2 3 4 5 6 7 8 9 10
            </div>
        </div>
    </div>
</template>

<script>
import Pin from '~/components/pin'

export default {
  name: 'Voter',
  components: { Pin },
  props: ['leftPin', 'rightPin'],
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>