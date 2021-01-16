<template>
  <div 
    class="quote-container" 
    @mouseenter="changePauseTimer(true)"
    @mouseleave="changePauseTimer(false)">
    <!-- Quote -->
    <div class="quote-text">
      <i class="fas fa-quote-left"></i>
      <span>{{ quote }}</span>
    </div>
    <!-- Author -->
    <div class="quote-author">
      <span>{{ author }}</span>
    </div>
    <!-- Buttons -->
    <div class="button-container">
      <button class="twitter-button" title="Tweet">
        <i class="fab fa-twitter"></i>
      </button>
      <button @click="fetchAllAndChoseOne()">Outra frase!</button>
    </div>
  </div>
</template>

<script>
export default {
  created() {
    this.fetchAllAndChoseOne();
    this.timer = setInterval(() => {
      if(!this.pauseTimer) {
        this.fetchAllAndChoseOne();
      }
    }, 5000);
  },
  beforeUnmount() {
    clearInterval(this.timer);
  },
  data() {
    return {
      quote: '',
      author: '',
      timer: null,
      pauseTimer: false
    }
  },
  methods: {
    fetchAllAndChoseOne () {
      fetch('http://localhost:3333/slogans')
      .then(response => response.json())
      .then(data => {
        const total = data.length;
        let quote = data[Math.floor(Math.random() * total)];
        this.quote = quote.body;
        this.author = quote.author;
      })
      .catch(console.error);
    },
    changePauseTimer(pause) {
      this.pauseTimer = pause;
    }
  }
}
</script>