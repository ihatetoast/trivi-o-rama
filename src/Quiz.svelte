<script>
  import { fade, slide, blur, fly, scale } from "svelte/transition";

  import Question from "./Question.svelte";
  import Modal from "./Modal.svelte";
  import { score } from "./store.js";
  // vars from Mars, Lars
  let activeQuestion = 0;
  let isModalOpen = false;
  // call quiz:
  let quiz = getQuiz();
  const url =
    "https://opentdb.com/api.php?amount=30&category=17&difficulty=medium&type=multiple";

  async function getQuiz() {
    const res = await fetch(url);
    const quiz = await res.json();
    return quiz;
  }

  function goToNext() {
    activeQuestion += 1;
  }
  function resetQuiz() {
    score.set(0);
    activeQuestion = 0;
    isModalOpen = false;
    quiz = getQuiz();
  }

  //REACTIVE STATEMENT / labelled statement
  // i don't have to check for the score every time!
  // and $ for autosubscription
  $: if ($score > 9) {
    isModalOpen = true;
  }
  //REACTIVE DECLARATION
  $: qNum = activeQuestion + 1;
</script>

<style lang="scss">
  .fade-wrapper {
    position: absolute;
  }
  .container {
    min-height: 500px;
  }
</style>

<div>
  <p>Need a do-over? You get one free reset. Choose wisely.</p>
  <button on:click|once={resetQuiz} class="doOver">Do over (1x)</button>
  <!-- $score is an autosubscription to the score obj -->
  <p class="score">score: {$score}/{activeQuestion}</p>
  <p class="questionCount">question: {qNum}</p>
</div>

<div class="container">
  {#await quiz}
    <p>Mining the depths of the trivia bank ...</p>
  {:then data}
    {#each data.results as questionData, idx}
      {#if idx === activeQuestion}
        <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
          <Question {questionData} {idx} {goToNext} />
        </div>
      {/if}
    {/each}

  {/await}
</div>
{#if isModalOpen}
  <Modal on:close={resetQuiz}>
    <!-- components with a slot can have things inside -->
    <h2 slot="modal">You got 10 correct!</h2>
    <p slot="modal">It took you x tries, making your final score y%.</p>
    <button slot="modal" on:click={resetQuiz}>Start over</button>
  </Modal>
{/if}
