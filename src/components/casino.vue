<template>
  <div class="game-container">
    <h2>Найди фото где: <br> {{ targetText }}</h2>
    <div class="grid" :class="{ shuffling: isShuffling }">
      <div
          v-for="(card, index) in cards"
          :key="index"
          class="card"
          :class="{ flipped: card.flipped, disabled: isShuffling }"
          @click="flipCard(index)">
        <div class="card-inner">
          <div class="card-front">?</div>
          <div class="card-back">
            <img :src="card.image" alt="Card Image" class="card-image" />
          </div>
        </div>
      </div>
    </div>

    <transition name="fade">
      <div v-if="showModal" class="modal-overlay">
        <transition name="scale">
          <div :class="modalClass" class="modal small-modal">
            <div class="test">
              <p class="test-title">Ты выбрал:</p>
              <img  :src="selectedImage" alt="Выбранное изображение" class="modal-image" />
            </div>
            <div class="test">
              <p class="test-title">Нужно было найти:</p>
              <img  :src="targetImage" alt="Целевая картинка" class="modal-image" />
            </div>
          </div>
        </transition>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';

// Массив изображений с подписями
const images = [
  { src: require('@/assets/vanya1.png'), text: 'Беник смотрит порно в маршрутке' },
  { src: require('@/assets/vanya2.png'), text: 'Беник спит с членом в жопе' },
  { src: require('@/assets/vanya3.png'), text: 'Беник плачет после секса с мужчиной' },
  { src: require('@/assets/vanya4.png'), text: 'Беник рассказывает сколько раз взял в рот (с Ленором и Фомой)' },
  { src: require('@/assets/vanya5.png'), text: 'Беник играет в ролевые игры (с Ленором и Фомой)' },
  { src: require('@/assets/vanya6.png'), text: 'Беник смотрит порно с ишаками' },
  { src: require('@/assets/vanya7.png'), text: 'Беник нюхает жопу Ленора' },
  { src: require('@/assets/vanya.png'), text: 'Беник под писающим фонтаном' }
];

// Перемешанные изображения
const shuffledImages = ref([...images].sort(() => Math.random() - 0.5));

// Целевая картинка и текст
const target = computed(() => shuffledImages.value[Math.floor(Math.random() * shuffledImages.value.length)]);
const targetImage = computed(() => target.value.src);
const targetText = computed(() => target.value.text);

// Карточки
const cards = ref(shuffledImages.value.map(image => ({ image: image.src, flipped: false })));

// Состояния модалки и игры
const showModal = ref(false);
const gameOver = ref(false);
const isShuffling = ref(false);
const selectedImage = ref('');
const isCorrect = ref(false);

// Функция для переворота карточки
const flipCard = (index: number) => {
  if (gameOver.value || cards.value[index].flipped || isShuffling.value) return;
  cards.value[index].flipped = true;
  gameOver.value = true;
  selectedImage.value = cards.value[index].image;

  // Проверка правильности выбора
  isCorrect.value = selectedImage.value === targetImage.value;

  showModal.value = true;

  setTimeout(() => {
    restartGame();
  }, 2000);
};

// Функция для перезапуска игры
const restartGame = () => {
  showModal.value = false;
  isShuffling.value = true;
  cards.value.forEach(card => card.flipped = false);

  setTimeout(() => {
    shuffledImages.value = [...images].sort(() => Math.random() - 0.5);
    cards.value = shuffledImages.value.map(image => ({ image: image.src, flipped: false }));
    gameOver.value = false;
    isShuffling.value = false;
    isCorrect.value = false; // Сбрасываем состояние угадал/не угадал
  }, 500);
};

// Отключаем выбор и прокрутку при старте игры
onMounted(() => {
  document.documentElement.style.userSelect = 'none';
  document.documentElement.style.touchAction = 'manipulation';
  document.documentElement.style.overflow = 'hidden';
});

// Класс модалки в зависимости от правильности выбора
const modalClass = computed(() => isCorrect.value ? 'modal-correct' : 'modal-incorrect');
</script>

<style scoped>

.test-img{
  height: 100%;
  width: auto;
}
.test-title{
height: 50px;
}
.test{
  height: 300px;
}

* {
  touch-action: manipulation;
  user-select: none;
}

.game-container {
  text-align: center;
  font-family: Arial, sans-serif;
  max-width: 100vw;
  overflow: hidden;
  padding: 10px;
  position: relative;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(130px, 1fr)); /* Сделано меньше */
  gap: 10px;
  justify-content: center;
  margin: 20px auto;
  max-width: 400px;
  transition: transform 1s ease-in-out;
}

.card {
  width: 100%;
  aspect-ratio: 1;
  perspective: 1000px;
  cursor: pointer;
  transition: transform 0.5s;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background: #fff;
}

.card.disabled {
  pointer-events: none;
}

.card-inner {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.5s;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  border-radius: 10px;
}

.card-front {
  background: linear-gradient(135deg, #ff8c00, #ff4500);
  color: white;
}

.card-back {
  background: white;
  transform: rotateY(180deg);
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}

.modal-image {
  width: 70%;
  min-height: 100px;
  min-width: 100px;
  max-width: 250px;
  margin: 10px 0;
  border-radius: 10px;
}

.small-modal {
  display: flex;
  justify-content: space-between;
  width: 60%;
  max-width: 280px;
  padding: 15px;
  background: white;
  position: fixed;
  top: 20%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.modal-overlay {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  opacity: 1;
  transition: opacity 0.3s ease-in-out;
}

.modal-correct {
  background-color: #d4edda; /* Светло-зеленый */
}

.modal-incorrect {
  background-color: #f8d7da; /* Светло-красный */
}
</style>
