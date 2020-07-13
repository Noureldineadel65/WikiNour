<script>
  import { createEventDispatcher, onMount } from "svelte";

  const dispatch = createEventDispatcher();
  let open = false;
  let searched = "";
  let clickHere = true;
  export let loadedMore = false;

  $: dispatch("search", searched);
  function toggleClosing() {
    open = !open;
    dispatch("open", open);
    clickHere = !open;
  }
  onMount(() => {
    if (searched) {
      dispatch("search", searched);
    }
  });
</script>

<style>
  .wiki-circle {
    width: 8rem;
    height: 8rem;
    border-radius: 50%;
    background-color: #fff;
    right: 50%;
    bottom: -50%;
    transform: translate(50%, 50%);
    cursor: pointer;
    transition: all 0.6s;
  }
  .opened {
    margin-top: -22.5rem;
  }
  .opened .wiki-circle {
    right: 2.5%;
    /* box-shadow: 0px 0px 16px 10px rgba(255, 255, 255, 1); */
  }
  .opened input {
    width: 100%;
    opacity: 1;
    /* box-shadow: 0px 0px 16px 10px rgba(255, 255, 255, 1); */
  }
  .wiki-circle img {
    width: 50%;
  }
  .search {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 70%;
  }
  input {
    font-size: 2rem;
    padding: 1rem 2rem;
    border-radius: 30px;
    width: 0%;
    opacity: 0;
    position: absolute;
    right: 50%;
    transform: translateX(50%);
    transition: all 0.6s;
    /* width: 100%; */
  }
  .click-here {
    font-size: 2rem;
    top: -12rem;
    opacity: 0.5;
    left: 50%;
    transform: translateX(-50%);
    animation: upanddown 0.6s infinite alternate linear;
  }
  @keyframes upanddown {
    0% {
      top: -12rem;
    }
    100% {
      top: -12.5rem;
    }
  }
</style>

<div class="search absolute flex items-center" class:opened={open}>
  <input
    type="text"
    id="search"
    placeholder="Search Encylopedia..."
    bind:value={searched} />
  {#if clickHere}
    <span class="click-here text-white absolute flex items-center flex-col">
      <span>Click Here</span>
      <img src="./images/arrow.svg" alt="" />
    </span>
  {/if}
  <div class="wiki-circle flex items-center justify-center absolute">
    {#if open}
      {#if searched}
        <img
          src="./images/close.svg"
          alt=""
          on:click={() => {
            searched = '';
            dispatch('clear');
          }} />
      {:else}
        <img src="./images/search.svg" alt="" on:click={toggleClosing} />
      {/if}
    {:else}
      <img src="./images/wiki.svg" alt="" on:click={toggleClosing} />
    {/if}
  </div>
</div>
