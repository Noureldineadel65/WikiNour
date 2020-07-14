<script>
  export let img = "";
  export let title = "";
  export let description = "";
  let pageContent = "";
  let pagePreview;
  function getPageContent() {
    pagePreview.parentElement.classList.add("active-container");
    fetch(
      `https://en.wikipedia.org/w/api.php?action=query&prop=extracts&exsentences=10&exlimit=1&titles=${title}&explaintext=1&formatversion=2&format=json&origin=*`
    )
      .then(blob => blob.json())
      .then(e => (pageContent = e.query.pages[0].extract));
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
</style>

{#if pageContent}
  <div class="pagePreview absolute">
    Lorem ipsum dolor sit, amet consectetur adipisicing elit. Harum, hic nemo
    iste quia maiores labore repellendus est nesciunt ab reiciendis perferendis
    excepturi et consequatur, ut eos exercitationem perspiciatis sunt nobis?
  </div>
{/if}
<li on:click={getPageContent} bind:this={pagePreview}>

  <!-- <a href={`https://en.wikipedia.org/wiki/${title}`} target="_blank"> -->
  <div class="title">{title}</div>
  <div class="description">{description}</div>
  <!-- </a> -->
</li>
