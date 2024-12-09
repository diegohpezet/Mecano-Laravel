<script setup>
import { ref, onMounted } from "vue";

const gameText = ref("Some random game text is available in the game text container");
const currentIndex = ref(0);
const userInput = ref("");
const isCompleted = ref(false);
const errorCount = ref(0);
const successRate = ref(0);

const resetGame = () => {
  currentIndex.value = 0;
  userInput.value = "";
  isCompleted.value = false;
  errorCount.value = 0;
};

const handleKeyPress = (event) => {
  const currentChar = gameText.value[currentIndex.value];

  // Do nothing when shift, enter, or caps lock is pressed
  if (event.key === "Shift" || event.key === "Enter" || event.key === "CapsLock" || event.key === "Backspace") {
    return;
  }

  // Check if the user inputs the correct character and update the game state accordingly
  if (!isCompleted.value) {
    if (event.key === currentChar) {
      userInput.value += event.key;
      currentIndex.value++;
    } else {
      errorCount.value++;
    }
  }

  // Check if the user completes the game
  if (currentIndex.value === gameText.value.length) {
    isCompleted.value = true;
    successRate.value = (gameText.value.length - errorCount.value) / gameText.value.length * 100;
  }
};

onMounted(() => {
  window.addEventListener("keydown", handleKeyPress);

  return () => {
    window.removeEventListener("keydown", handleKeyPress);
  };
});
</script>

<template>
  <section name="gameBoard" class="border rounded bg-light p-3">

    <!-- Game Text -->
    <p class="fs-3">
      <span v-for="(char, index) in gameText" :key="index">
        <span :class="{
          currentLetter: index === currentIndex,
          correct: index < currentIndex && userInput[index] === char,
          incorrect: index < currentIndex && userInput[index] !== char,
        }">
          {{ char }}
        </span>
      </span>
    </p>

    <!-- Game stats -->
    <div class="text-end">
      <p class="fw-bold">{{ currentIndex }} / {{ gameText.length }}</p>
      <p class="text-danger fw-bold">Errors: {{ errorCount }}</p>
    </div>

    <!-- Game status -->
    <div class="text-center text-muted">
      <p v-if="!isCompleted">Keep typing...</p>
      <p v-else>🎉 Congratulations! You've completed the game!. ({{ successRate.toFixed(1) }}% accuracy)</p>
    </div>

    <!-- Actions -->
    <div class="text-center">
      <button @click="resetGame" class="btn btn-primary">Reset Game</button>
    </div>
  </section>
</template>

<style scoped>
.currentLetter {
  color: red;
  text-decoration: underline;
}

.correct {
  color: green;
}

.incorrect {
  color: orange;
}
</style>
