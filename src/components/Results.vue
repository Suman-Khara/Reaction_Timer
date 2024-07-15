<template>
  <div>
    <p>Reaction time: {{ score }} ms</p>
    <p class="rank">{{ rank }}</p>
    <p class="comment">{{ comment }}</p>
  </div>
</template>

<script>
export default {
  props: ['score'],
  data() {
    return {
      rank: null,
      comment: ''
    }
  },
  mounted() {
    this.calculateRankAndComment()
  },
  methods: {
    calculateRankAndComment() {
      if (this.score < 400) {
        this.rank = 'Ninja Fingers'
        this.comment = 'Wow! You have lightning-fast reflexes!'
      } else if (this.score < 800) {
        this.rank = 'Rapid Reflexes'
        this.comment = 'Great job! You reacted quickly!'
      } else {
        this.rank = 'Slowpoke'
        this.comment = 'Keep practicing! You can improve your reaction time.'
      }
    }
  }
}
</script>

<style>
.rank {
  font-size: 1.4em;
  color: #0faf87;
  font-weight: bold;
  font-family: 'Press Start 2P', cursive;
}

.comment {
  margin-top: 20px;
  font-size: 1.2em;
  color: #777;
}
</style>