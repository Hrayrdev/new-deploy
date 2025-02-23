<template>
  <div class="game-container" :style="containerStyle">
    <div v-if="modalVisible" class="modal">
      <div class="modal-content">
        <p>{{ modalMessage }}</p>
        <button @click="restartGame">Restart</button>
      </div>
    </div>

    <div v-if="!modalVisible" class="task">
      <p>Find the card with the symbol: <strong>{{ targetSymbol }}</strong></p>
    </div>

    <div class="grid">
      <div
          v-for="(card, index) in cards"
          :key="index"
          class="card"
          :class="{ 'flipped': card.flipped, 'disabled': gameOver }"
          @click="flipCard(card, index)"
      >
        <div class="front"></div>
        <div class="back">{{ card.symbol }}</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const cards = ref<Array<{ symbol: string, flipped: boolean }>>([]);
const targetSymbol = ref<string>('');
const modalVisible = ref(false);
const modalMessage = ref('');
const gameOver = ref(false);

const containerStyle = {
  'max-width': '100vw',
  'max-height': '100vh',
  'overflow': 'hidden',
  'touch-action': 'none',  // предотвращает зум на мобильных устройствах
};

const symbols = ['★', '♦', '♠', '♥', '♦', '♠', '♥', '★', '♣'];

const shuffleCards = () => {
  cards.value = symbols.map(symbol => ({ symbol, flipped: false }));
  cards.value = [...cards.value].sort(() => Math.random() - 0.5);
};

const flipCard = (card: { symbol: string, flipped: boolean }, index: number) => {
  if (gameOver.value || card.flipped) return;

  card.flipped = true;

  if (card.symbol === targetSymbol.value) {
    modalMessage.value = 'You Win!';
    modalVisible.value = true;
  } else if (cards.value.every(card => card.flipped)) {
    modalMessage.value = 'Game Over!';
    modalVisible.value = true;
  }
};

const restartGame = () => {
  modalVisible.value = false;
  gameOver.value = false;
  shuffleCards();
  targetSymbol.value = symbols[Math.floor(Math.random() * symbols.length)];
  setTimeout(() => {
    cards.value.forEach(card => card.flipped = false);
  }, 1500);
};

onMounted(() => {
  shuffleCards();
  targetSymbol.value = symbols[Math.floor(Math.random() * symbols.length)];
});
</script>

<style scoped>
.game-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  flex-direction: column;
  font-family: 'Arial', sans-serif;
  background-color: #f3f3f3;
}

.task {
  font-size: 1.5rem;
  margin-bottom: 20px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.card {
  position: relative;
  width: 100px;
  height: 100px;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  transform-style: preserve-3d;
  transition: transform 0.5s;
}

.card.flipped {
  transform: rotateY(180deg);
}

.card .front, .card .back {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  border-radius: 10px;
  font-size: 2rem;
  color: #333;
}

.card .back {
  transform: rotateY(180deg);
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  animation: fadeIn 0.3s forwards;
}

.modal-content {
  background-color: white;
  padding: 20px;
  text-align: center;
  border-radius: 10px;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

.disabled {
  pointer-events: none;
}

body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  height: 100%;
}

html {
  touch-action: none; /* Отключение приближения */
}
</style>
