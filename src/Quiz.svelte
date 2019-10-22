<script>
  import Question from "./Question.svelte";
  // call quiz:
  let quiz = getQuiz();
  const url =
    "https://opentdb.com/api.php?amount=10&category=17&difficulty=medium&type=multiple";

  async function getQuiz() {
    const res = await fetch(url);
    quiz = await res.json();
    console.log(quiz);
    return quiz;
  }

  function handleClick() {
    quiz = getQuiz();
  }
</script>

<style>

</style>

<div>

  <button on:click={handleClick()}>Get quiz</button>

</div>
<div>
  {#await quiz}
    <p>Mining the depths of the trivia bank ...</p>
  {:then data}
    {#each data.results as ques, index}
      <Question {ques} {index} />
    {/each}

  {/await}
</div>
