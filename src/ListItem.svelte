<script>
  export let img = "";
  export let title = "";
  export let description = "";

  function fetchContent() {
    visible = true;
    fetch(
      `https://en.wikipedia.org/w/api.php?action=query&prop=extracts&exsentences=10&exlimit=1&titles=${title}&explaintext=1&formatversion=2&format=json&origin=*`
    )
      .then(blob => blob.json())
      .then(e => {
        pageContent = e.query.pages[0].extract;
      });
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
  li:hover {
    transform: scale(1.025);
  }
  .thumbnail img {
    width: 100%;
    object-fit: cover;
  }
  .thumbnail {
    grid-row: 1/-1;
    grid-column: 1/2;
    justify-self: center;
    width: 5rem;
    height: 5rem;
  }
  .description {
    font-size: 1.3rem;
  }
  .page-view {
    font-size: 1.4rem;
    padding: 2rem;
    position: absolute;
    background-color: #fff;
    z-index: 10;
    width: 100%;
    height: 100%;
    top: 0;
    bottom: 0;
  }
  .page-view img {
    width: 100%;
    margin: 0 0 2rem 0;
  }
  .page-view p {
  }
</style>

<li on:click={fetchContent}>

  <div class="title">{title}</div>
  <div class="description">{description}</div>
</li>
