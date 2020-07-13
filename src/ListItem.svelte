<script>
  export let img = "";
  export let title = "";
  export let description = "";
  let pageContent = "";
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
    font-size: 1.6rem;
    width: 40rem;
    right: 0;
    top: 0;

    padding: 2rem;
  }
</style>

<li>

  <a href={`https://en.wikipedia.org/wiki/${title}`} target="_blank">
    <div class="title">{title}</div>
    <div class="description">{description}</div>
  </a>
</li>
