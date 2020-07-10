<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  let open = false;
  let searched = "";
  let results = [];
  let gsroffset = 0;
  function getResults(search) {
    if (search) {
      fetch(
        `https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&gsrlimit=20&prop=pageimages|extracts&exintro&explaintext&exsentences=1&exlimit=max&gsrsearch=${search}&origin=*`
      )
        .then(blob => blob.json())
        .then(e => {
          gsroffset = e.continue.gsroffset;
          results = Object.values(e.query.pages);
          dispatch("searched", results);
        });
    }
  }
  $: getResults(searched);
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
    right: 0%;
  }
  .opened input {
    width: 100%;
    opacity: 1;
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
</style>

<div class="search absolute flex items-center" class:opened={open}>
  <input
    type="text"
    id="search"
    placeholder="Search Encylopedia..."
    bind:value={searched} />
  <div
    class="wiki-circle flex items-center justify-center absolute"
    on:click={() => (open = !open)}>
    <img src="./images/wiki.svg" alt="" />
  </div>
</div>
