<script>
  import Nav from "./Nav.svelte";
  import Search from "./Search.svelte";
  import ListItem from "./ListItem.svelte";
  let listItems = [];
  $: console.log(listItems);
</script>

<style>
  .container {
    width: 70%;
    margin: 0 auto;
  }
</style>

<main>
  <Nav />
  <div class="container">
    <Search on:searched={e => (listItems = e.detail)} />
    {#each listItems as item (item.pageid)}
      <ListItem
        img={item.thumbnail ? item.thumbnail.source : ''}
        title={item.title}
        description={item.extract} />
    {/each}

  </div>
</main>
