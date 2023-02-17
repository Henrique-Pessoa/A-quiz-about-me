<script setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question: "When's my birthday",
    answer: 0,
    select: null,
    options: ["07/24", "07/25", "10/21", "04/10"],
  },
  {
    question: "What's my age",
    answer: 1,
    select: null,
    options: ["17", "18", "19", "54"],
  },
  {
    question:
      "What year did i start learn programming? and with which language? ",
    answer: 0,
    select: null,
    options: [
      "2020 with python",
      "2020 with java",
      "2022 with react",
      "2022 with javascript,html and css",
    ],
  },
  {
    question: "What's my favorite anime",
    answer: 2,
    select: null,
    options: ["Naruto", "Tokyo ghoul", "Gosick", "Dragon ball Z"],
  },
  {
    question: "What's my favorite language?",
    answer: 1,
    select: null,
    options: ["Vue", "typescript", "React", "Python"],
  },
]);

const quizComplete = ref(false);

const currentQuestions = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.select == q.answer) {
      value++;
    }
  });
  return value;
});

const getQuestion = computed(() => {
  let question = questions.value[currentQuestions.value];
  question.index = currentQuestions.value;
  return question;
});

const setAnswer = (e) => {
  questions.value[currentQuestions.value].select = e.target.value;
  e.target.value = null;
};

const nextQuestion = () => {
  if (currentQuestions.value < questions.value.length - 1) {
    currentQuestions.value++;
    return console.log(currentQuestions.value);
  }
  quizComplete.value = true;
};
const backQuestion = () => {
  if (
    currentQuestions.value !=
    questions.value.length - questions.value.length
  ) {
    currentQuestions.value--;
  }
};

const tryAgain = () => {
  quizComplete.value = false;
  currentQuestions.value = 0;
};
</script>

<template>
  <div class="main">
    <div v-if="!quizComplete">
      <h1>A quiz about me</h1>
    </div>
    <section class="quiz" v-if="!quizComplete">
      <div class="quizInfo">
        <span class="question">{{ getQuestion.question }}</span>
        <span class="questionNumber">
          Questions : {{ currentQuestions + 1 }} / {{ questions.length }}</span
        >
      </div>
      <div class="options">
        <label
          v-for="(option, index) in getQuestion.options"
          :key="index"
          class="option"
        >
          <input
            class="checkBox"
            type="radio"
            :id="'option' + index"
            :name="getQuestion.index"
            :value="index"
            v-model="getQuestion.select"
            @change="setAnswer"
          />
          <span class="checkMark">{{ option }}</span>
        </label>
      </div>

      <button
        :class="`nextQuestion ${
          getQuestion.select == null ? 'cantUse' : 'canUse'
        }`"
        @click="nextQuestion"
        :disabled="!getQuestion.select"
      >
        {{
          getQuestion.index == questions.length - 1
            ? "Finish"
            : getQuestion.select == null
            ? "Select a answare"
            : "Next"
        }}
      </button>
      <button
        :class="`backQuestion ${getQuestion.index > 0 ? 'canUse' : 'cantUse'}`"
        :disabled="getQuestion.index == 0"
        @click="backQuestion"
      >
        Back
      </button>
    </section>

    <section v-else>
      <h2>You have finish the quiz</h2>
      <p class="scoreResult">You score's: {{ score }}/{{ questions.length }}</p>
      <div class="scoreReaction">
        <p v-if="score == 0">I guess that you don't know me &#128542;</p>
        <p v-else-if="score > 0 < 3">you know me a little</p>
        <p v-else>you really know me</p>
        <button class="try" @click="tryAgain">want to try again?</button>
      </div>
    </section>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed:ital,wght@1,700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Inconsolata:wght@600&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@500&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #8890ea;
}
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}
h1 {
  position: relative;
  font-family: "Gill Sans", sans-serif;
  font-size: 100;
  color: #fff;
  animation-name: enter;
  animation-duration: 5s;
  margin-bottom: 2rem;
}

@keyframes enter {
  from {
    opacity: 0;
  }
  to {
    opacity: 100;
  }
}
.quiz {
  background-color: #4e5891;
  border-radius: 10%;
  width: 60rem;
  height: 30rem;
  box-shadow: 7px 13px 6px -1px rgba(0, 0, 0, 0.75);
  padding: 2rem;
}
.quizInfo {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.question {
  font-family: cursive;
  color: #fff;
  font-size: 1.3rem;
}
.questionNumber {
  color: #141624;
  font-size: 1.5rem;
  font-family: "Roboto Condensed", sans-serif;
}
.score {
  position: relative;
  font-size: 1.5rem;
  right: 4rem;
}
.checkMark {
  font-family: "Roboto Condensed", sans-serif;
}
.options {
  margin-bottom: 1rem;
}
.option {
  display: block;
  padding: 1.25rem;
  background-color: #3e4673;
  cursor: pointer;
  margin-bottom: 0.5rem;
  margin-top: 0.5rem;
  border-radius: 10px;
  width: 35rem;
}

.option:hover {
  transform: scale(1.1);
  background-color: #1f2645;
}
input {
  height: 1.5rem;
  width: 1.5rem;
}
span {
  color: black;
  font-family: system-ui;
  font-size: 1.25rem;
  margin-left: 1.5rem;
}
button {
  font-family: Oldtown, fantasy;
  font-size: 1rem;
}

.nextQuestion {
  appearance: none;
  outline: none;
  border: none;
  padding: 1rem 2rem;
  border-radius: 2rem;
  box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
  color: black;
}

.nextQuestion.canUse {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
  background-color: #0fffbd;
}
.nextQuestion.canUse:hover {
  transform: scale(1.1);
  cursor: pointer;
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
.nextQuestion.cantUse {
  background-color: #962146;
  text-decoration: line-through;
}
.backQuestion {
  position: relative;
  appearance: none;
  outline: none;
  border: none;
  left: 5rem;
  padding: 1rem 2rem;
  border-radius: 2rem;
  box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
  color: black;
}
.backQuestion.canUse {
  animation: shakeBack 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  background-color: #0fffbd;
}
.backQuestion.canUse:hover {
  transform: scale(1.1);
  cursor: pointer;
}

@keyframes shakeBack {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}

.backQuestion.cantUse {
  background-color: #962146;
  text-decoration: line-through;
}
h2 {
  margin-top: 0px;
  margin-bottom: 50px;
  text-align: center;
  font-family: sans-serif;
  font-size: 4rem;
  letter-spacing: 0.15rem;
  text-transform: uppercase;
  color: #fff;
  text-shadow: -4px 4px #ef3550, -8px 8px #f48fb1, -12px 12px #7e57c2,
    -16px 16px #2196f3, -20px 20px #26c6da, -24px 24px #43a047,
    -28px 28px #eeff41, -32px 32px #f9a825, -36px 36px #ff5722;
  animation-name: enter;
  animation-duration: 5s;
}
.scoreResult {
  font-size: 2rem;
  text-align: center;
  margin-top: 1rem;
  margin-bottom: 1rem;
  font-family: "Inconsolata", monospace;
}
.scoreReaction {
  font-size: 1.6rem;
  font-family: "Josefin Sans", sans-serif;
  text-align: center;
}
.try {
  appearance: none;
  outline: none;
  border: none;
  padding: 1rem 2rem;
  border-radius: 2rem;
  box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
  margin-top: 2rem;
  color: black;
  background-color: #67648c;
}
.try:hover {
  background-color: #0fffbd;
  cursor: pointer;
  animation-name: event;
  animation-duration: 1s;
}
@keyframes event {
  from {
  }
  to {
    transform: scale(1.1);
  }
}
</style>
