<script setup>
</script>

<template>
  <div id="app">
    <h1>Sentiment Analysis</h1>
    <textarea
      v-model="userInput"
      placeholder="Enter text for analysis..."
    ></textarea>
    <button @click="analyzeSentiment">Analyze</button>
    <div v-if="sentiment" class="result">
      <h2>Analysis Result:</h2>
      <div :class="['sentiment-box', sentimentClass]">
        <p>Sentiment: <strong>{{ sentiment.label.toLowerCase }}</strong></p>
        <p>Score: <strong>{{ sentiment.score.toFixed(2) }}</strong></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      userInput: '',
      sentiment: null
    }
  },
  computed: {
    sentimentClass() {
      if (!this.sentiment) return '';
      if (this.sentiment.label === 'positive') return 'positive';
      if (this.sentiment.label === 'negative') return 'negative';
      return 'neutral';
    }
  },
  methods: {
    async analyzeSentiment() {
      try {
        const response = await fetch("https://cc-backend-git-cloud-computing-2025-course.2.rahtiapp.fi/sentiment", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ text: this.userInput }),
        });
        this.sentiment = await response.json();
      } catch (error) {
        console.error("Error analyzing sentiment:", error);
      }
    },
  },
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #4CAF50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
}

.result {
  margin-top: 20px;
}

.sentiment-box {
  padding: 10px;
  border-radius: 4px;
  margin-top: 10px;
}
.positive {
  background-color: #d4edda;
  color: #155724;
}

.negative {
  background-color: #f8d7da;
  color: #721c24;
}

.neutral {
  background-color: #e2e3e5;
  color: #383d41;
}

@media (max-width: 600px) {
  #app {
    padding: 10px;
  }

  button {
    width: 100%;
  }
}
</style>