<template>
  <b-row>
    <div class="text-center c404 w-100 d-flex flex-column justify-content-center align-items-center">
      <div @mousemove="bubble" class="w-100 h-100 position-absolute"/>
      <img class="py-5" src="/img/fish.svg"/>
      <h1 class="n-blue font-weight-bolder">Vous vous êtes perdus en mer</h1>
      <h2 class="n-blue font-weight-light text-404">~ blueblue ~</h2>
      <router-link to="/" class="pt-5 back-btn">
        <Button>
          Rejoindre le rivage
        </Button>
      </router-link>
      <div
      touch-action="none"
      class="bubble n-bg-blue position-absolute"
      v-for="(bubble, index) in bubbles"
      :key="index"
      :style="{
        width: `${bubble.radius}px`,
        height: `${bubble.radius}px`,
        opacity: bubble.opacity,
        top: `${bubble.y}px`,
        left: `${bubble.x}px`
      }"/>
    </div>
  </b-row>
</template>

<script>
import Button from '../components/Button'

export default {
  data: () => ({
    bubbles: [],
    ticks: 0
  }),
  mounted () {
    setInterval(() => {
      this.bubbles.map((bubble, index) => {
        const newBubble = {
          ...bubble,
          radius: bubble.radius + 0.09,
          opacity: bubble.opacity - 0.003,
          y: bubble.y - 1
        }
        if (newBubble.opacity > 0) {
          this.$set(this.bubbles, index, newBubble)
        } else {
          this.bubbles.splice(index, 1)
        }
      })
      this.ticks++
    }, 0)
  },
  methods: {
    bubble (event) {
      if (this.ticks >= 30) {
        const radius = Math.random() * (40 - 20) + 20
        this.bubbles.push({
          radius,
          opacity: Math.random() * (1 - 0.2) + 0.2,
          x: event.clientX,
          y: event.clientY
        })
        this.ticks = 0
      }
    }
  },
  components: {
    Button
  }
}
</script>

<style lang="scss" scoped>
.bubble {
  touch-action: none;
  pointer-events: none;
  border-radius: 100%;
  z-index: -9;
}

.back-btn {
  z-index: 999;
}

img {
  width: 60px;
}

.c404 {
  position: relative;
  height: 100vh;
  overflow: hidden;
}

.text-404 {
  font-size: 1.5em;
}
</style>
