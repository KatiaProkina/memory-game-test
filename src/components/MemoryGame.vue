<template>
  <div class="game-container">
    <div class="game-board">
      <div
        class="card"
        v-for="card in cards"
        :key="card.id"
        @click="flipCard(card)">
        <div
          class="card-inner"
          :class="{ flipped: card.isFlipped || card.isMatched }">
          <div class="card-front">
            <img :src="card.image" alt="Card Image" />
          </div>
          <div class="card-back"></div>
        </div>
      </div>
    </div>
    <button class="restart-button" @click="initializeGame">
      Перезапустить игру
    </button>
  </div>
</template>
<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import './MemoryGame.css';

interface Card {
  id: number;
  image: string;
  isFlipped: boolean;
  isMatched: boolean;
}

import image1 from '../assets/image1.png';
import image2 from '../assets/image2.png';
import image3 from '../assets/image3.png';
import image4 from '../assets/image4.png';
import image5 from '../assets/image5.png';
import image6 from '../assets/image6.png';
import image7 from '../assets/image7.png';
import image8 from '../assets/image8.gif';
import image9 from '../assets/image9.png';
import image10 from '../assets/image10.png';
import image11 from '../assets/image11.png';
import image12 from '../assets/image12.png';
import image13 from '../assets/image13.png';
import image14 from '../assets/image14.png';
import image15 from '../assets/image15.gif';

const images = [
  image1,
  image2,
  image3,
  image4,
  image5,
  image6,
  image7,
  image8,
  image9,
  image10,
  image11,
  image12,
  image13,
  image14,
  image15,
];

const cards = ref<Card[]>([]);
const selectedCards = ref<Card[]>([]);

const createCards = (): Card[] => {
  let id = 1;
  const newCards: Card[] = images.flatMap((image) => {
    return [
      { id: id++, image, isFlipped: false, isMatched: false },
      { id: id++, image, isFlipped: false, isMatched: false },
    ];
  });
  return shuffle(newCards);
};

const shuffle = (array: Card[]): Card[] => {
  return array.sort(() => Math.random() - 0.5);
};

const initializeGame = () => {
  cards.value = createCards();
  showAllCards();
};

const showAllCards = () => {
  cards.value.forEach((card) => (card.isFlipped = true));
  setTimeout(() => {
    cards.value.forEach((card) => (card.isFlipped = false));
  }, 3000);
};

const flipCard = (card: Card) => {
  if (card.isMatched || selectedCards.value.length >= 2 || card.isFlipped) {
    return;
  }
  card.isFlipped = true;
  selectedCards.value.push(card);

  if (selectedCards.value.length === 2) {
    checkForMatch();
  }
};

const checkForMatch = () => {
  const [firstCard, secondCard] = selectedCards.value;
  if (firstCard.image === secondCard.image) {
    firstCard.isMatched = true;
    secondCard.isMatched = true;
    selectedCards.value = [];
    checkForWin();
  } else {
    setTimeout(() => {
      firstCard.isFlipped = false;
      secondCard.isFlipped = false;
      selectedCards.value = [];
    }, 1000);
  }
};

const checkForWin = () => {
  if (cards.value.every((card) => card.isMatched)) {
    setTimeout(() => {
      alert('Поздравляем! Вы нашли все пары.');
      // initializeGame();
    }, 500);
  }
};

onMounted(() => {
  initializeGame();
});
</script>
