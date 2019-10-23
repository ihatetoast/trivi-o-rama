<script>
  import { fly, fade } from "svelte/transition";

  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
</script>

<style lang="scss">
  .modalWrapper {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(10, 10, 10, 0.9);
    .modal {
      background: #fff;
      padding: 1em;
      border-radius: 8px;
      width: 30em;
      margin: 0 auto;
      margin-top: 3em;
    }
  }
</style>

<div class="modalWrapper" transition:fade>
  <div class="modal" in:fly={{ y: 500 }} out:fly={{ y: -300 }}>
    <button
      on:click={() => {
        dispatch('close');
      }}>
      X
    </button>
    <slot name="modal" />
    <!-- <slot>optional fallback </slot>-->
    <!-- <slot /> generic -->

  </div>
</div>
