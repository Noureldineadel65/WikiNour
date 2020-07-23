<script>
  import { scale } from "svelte/transition";

  export let img = "";
  export let title = "";
  export let description = "";
  let pageContent = "";
  let displayPreview = false;
  let pagePreview;
  function getPageContent() {
    displayPreview = true;
    pagePreview.parentElement.classList.add("active-container");
    if (!pageContent) {
      fetch(
        `https://en.wikipedia.org/w/api.php?action=query&prop=extracts&exsentences=10&exlimit=1&titles=${title}&explaintext=1&formatversion=2&format=json&origin=*`
      )
        .then(blob => blob.json())
        .then(e => {
          pageContent = e.query.pages[0].extract;
        });
    }
  }
</script>

<style>
  .title {
    font-size: 1.6rem;
  }
  li {
    display: flex;
    flex-direction: column;
    background-color: #fff;
    margin-bottom: 2rem;
    padding: 1rem;
    cursor: pointer;
    transition: all 0.3s;
    box-shadow:
    /* The top layer shadow */ 0 1px 1px rgba(0, 0, 0, 0.15),
      /* The second layer */ 0 10px 0 -5px #eee,
      /* The second layer shadow */ 0 10px 1px -4px rgba(0, 0, 0, 0.15),
      /* The third layer */ 0 20px 0 -10px #eee,
      /* The third layer shadow */ 0 20px 1px -9px rgba(0, 0, 0, 0.15);
  }

  .description {
    font-size: 1.3rem;
  }
  .pagePreview {
    background-color: #fff;
    font-size: 1.4rem;
    width: 40rem;
    right: 0;
    top: 0;

    padding: 2rem;
  }
  .pagePreview img {
    width: 21rem;
  }
  li:hover .title {
    text-decoration: underline;
  }
</style>

<li
  on:mouseenter={getPageContent}
  bind:this={pagePreview}
  on:mouseleave={() => {
    displayPreview = false;
    pagePreview.parentElement.classList.remove('active-container');
  }}
  on:click={() => {
    window.open(`https://en.wikipedia.org/wiki/${title}`, '_blank');
  }}>

  <!-- <a href={`https://en.wikipedia.org/wiki/${title}`} target="_blank"> -->
  <div class="title">{title}</div>
  <div class="description">
    {pageContent && displayPreview ? pageContent.slice(0, 700) + '...' : description}
  </div>
  <!-- </a> -->
</li>
