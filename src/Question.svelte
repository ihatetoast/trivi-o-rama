<script>
  import { fade, slide, blur, fly, scale } from "svelte/transition";
  import { score } from "./store.js";
  // props:
  export let questionData;
  export let idx;
  export let goToNext;
  //vars from mars
  let isCorrect;
  let isAnswered = false;
  let answers = questionData.incorrect_answers.map(answer => {
    return {
      answer: answer,
      correct: false
    };
  });
  let allAnswers = [
    ...answers,
    {
      answer: questionData.correct_answer,
      correct: true
    }
  ];
  const hurrays = [
    "Saweeet!",
    "You a smarty Marty.",
    "That's Dr. Smartypants to you.",
    "ERMAGERD, YER SMERT!",
    "Huzzzah!"
  ];
  const boos = [
    "Lame...",
    "Don't know much, do you?",
    "Yawn...",
    "ERMAGERD, YER DERM!",
    "Honestly, mate. You know nuthin'!"
  ];
  shuffle(allAnswers);
  function shuffle(arr) {
    arr.sort(() => Math.random() - 0.5);
  }
  function getComment(arr) {
    shuffle(arr);
    console.log(arr);
    return arr[0];
  }
  function checkAnswer(correct) {
    if (!isAnswered) {
      isAnswered = true;
      isCorrect = correct;
      if (correct) {
        score.update(val => val + 1);
      }
    }
  }
</script>

<style lang="scss">
  .answerBtn {
    display: block;
  }
  p.isCorrect {
    color: rebeccapurple;
  }
  p.isWrong {
    color: red;
  }
</style>

<h3>
  {@html questionData.question}
  {#if isAnswered}
    <p class:isCorrect class:isWrong={!isCorrect} transition:fade>
      {#if isCorrect}{getComment(hurrays)}{:else}{getComment(boos)}{/if}
    </p>
  {/if}
</h3>

<div>
  {#each allAnswers as answer}
    <button
      class="answerBtn"
      disabled={isAnswered}
      on:click={() => checkAnswer(answer.correct)}>
      {@html answer.answer}
    </button>
  {/each}
</div>

{#if isAnswered}
  <div>
    <button transition:fade on:click={goToNext}>Next question</button>
  </div>
{/if}
