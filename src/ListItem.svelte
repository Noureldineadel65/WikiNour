<script>
  export let img = "";
  export let title = "";
  export let description = "";
</script>

<style>
  .title {
    font-size: 1.6rem;
  }
  li {
    display: grid;
    grid-template-columns: repeat(2, auto);
    grid-template-rows: repeat(2, auto);
    grid-column-gap: 2rem;
    align-items: center;
  }
  .thumbnail {
    grid-row: 1/-1;
    grid-column: 1/2;
  }
  .description {
    font-size: 1.3rem;
  }
</style>

<li class="text-white">
  <div class="thumbnail">
    <img src={img} alt="" />
  </div>
  <div class="title">{title}</div>
  <div class="description">{description}</div>
</li>
