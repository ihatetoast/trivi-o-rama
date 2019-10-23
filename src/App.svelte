<script>
  import Quiz from "./Quiz.svelte";
</script>

<style>
  :global(body) {
    text-align: center;
    background: rebeccapurple;
  }
  h1 {
    color: #fff;
  }
  .quiz-wrapper {
    background: white;
    border-radius: 15px;
    padding: 20px 40px;
  }
</style>

<h1>IHateToast's Trivi-o-rama</h1>
<div class="quiz-wrapper">
  <Quiz />
</div>
