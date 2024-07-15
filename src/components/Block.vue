<template>
  <div 
    class="block" 
    v-if="showBlock" 
    :style="blockStyle"
    @click="stopTimer"
  >
    click me
  </div>
</template>

<script>
export default {
  props: ['delay'],
  data() {
    return {
      showBlock: false,
      startTimestamp: null,
      reactionTime: 0,
      blockStyle: {
        top: '0px',
        left: '0px',
        width: '100px',
        height: '100px',
        backgroundColor: '#0faf87',
        color: '#ffffff'
      }
    }
  },
  mounted() {
    console.log('component mounted')
    setTimeout(() => {
      this.randomizeStyle()
      this.showBlock = true
      this.startTimer()
    }, this.delay)
  },
  beforeUnmount() {
    clearInterval(this.timer)
    cancelAnimationFrame(this.timer)
    console.log('component unmounted')
  },
  methods: {
    randomizeStyle() {
      const screenWidth = window.innerWidth
      const screenHeight = window.innerHeight
      const size = 50 + Math.random() * 150
      const top = Math.random() * (screenHeight - size)
      const left = Math.random() * (screenWidth - size)

      const r = Math.floor(Math.random() * 256)
      const g = Math.floor(Math.random() * 256)
      const b = Math.floor(Math.random() * 256)
      const backgroundColor = `rgb(${r}, ${g}, ${b})`

      const textColor = this.getComplementaryColor(r, g, b)

      this.blockStyle = {
        top: `${top}px`,
        left: `${left}px`,
        width: `${size}px`,
        height: `${size}px`,
        backgroundColor: backgroundColor,
        color: textColor
      }
    },
    getComplementaryColor(r, g, b) {
      const complementaryColor = `rgb(${255 - r}, ${255 - g}, ${255 - b})`
      const brightness = (r * 299 + g * 587 + b * 114) / 1000
      const textColor = brightness > 128 ? '#000000' : '#ffffff'
      return textColor
    },
    startTimer() {
      this.startTimestamp = performance.now()
      this.timer = requestAnimationFrame(this.updateTime)
    },
    updateTime() {
      const currentTime = performance.now()
      this.reactionTime = Math.round(currentTime - this.startTimestamp)
      this.timer = requestAnimationFrame(this.updateTime)
    },
    stopTimer() {
      cancelAnimationFrame(this.timer)
      console.log(this.reactionTime)
      this.$emit('end', this.reactionTime)
    }
  }
}
</script>

<style>
.block {
  position: absolute;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: 'Press Start 2P', cursive;
  cursor: pointer;
}
</style>
