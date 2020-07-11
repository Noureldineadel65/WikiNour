<script>
  import Nav from "./Nav.svelte";
  import { slide, fly, scale } from "svelte/transition";
  import Search from "./Search.svelte";
  import ListItem from "./ListItem.svelte";
  let listItems = [];
  let canvas;
  let open = false;
  let loadedMore = false;
  let search = "";
  let gsroffset = 0;
  $: getResults(search);
  function getResults(search) {
    if (search) {
      if (!loadedMore) {
        fetch(
          `https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&prop=pageimages|extracts&exintro&explaintext&exsentences=1&exlimit=max&gsrsearch=${search}&origin=*&pithumbsize=400`
        )
          .then(blob => blob.json())
          .then(e => {
            gsroffset = e.continue.gsroffset;
            listItems = Object.values(e.query.pages);
          });
      } else {
        fetch(
          `https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&prop=pageimages|extracts&exintro&explaintext&exsentences=1&exlimit=max&gsrsearch=${search}&origin=*&pithumbsize=400&gsroffset=${gsroffset}`
        )
          .then(blob => blob.json())
          .then(e => {
            gsroffset = e.continue.gsroffset;
            listItems = [...listItems, ...Object.values(e.query.pages)];
            console.log(listItems);
            // console.log([...listItems, Object.values(e.query.pages)]);
            loadedMore = false;
            // listItems = [...Object.values(e.query.pages)];
          });
      }
    } else {
      listItems = [];
    }
  }
</script>

<style>
  .container {
    width: 70%;
    margin: 0 auto;
  }
  .list-items {
    margin-top: 17.5rem;
  }
  .load-more {
    color: #222;
    background-color: #fff;
    font-size: 2rem;
    padding: 1rem 2rem;
    margin: 2rem 0 4rem 0;
  }
  .book {
    font-size: 2.1rem;
    color: #fff;

    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .book img {
    width: 10rem;
  }
  .book p {
    margin-top: -10rem;
  }
</style>

<main>
  <Nav />
  <div class="container">
    <Search
      on:search={e => (search = e.detail)}
      on:open={e => (open = e.detail)}
      {loadedMore} />
    {#if open}
      {#if listItems.length}
        <div class="list-items" transition:fly={{ x: -200, duarion: 300 }}>
          {#each listItems as item (item.index)}
            <div class="list-item-container" transition:slide|local>
              <ListItem
                img={item.thumbnail ? item.thumbnail.source : ''}
                title={item.title}
                description={item.extract} />
            </div>
          {/each}

        </div>
        {#if gsroffset !== 400}
          <div class="text-center">
            <button
              class="load-more mx-auto"
              on:click={() => {
                loadedMore = true;
                getResults(search);
              }}>
              Load More
            </button>
          </div>
        {/if}
      {:else}
        <div class="book" transition:scale={{ duration: 300 }}>
          <p>Search for Articles</p>
          <img src="./images/book.svg" alt="" class="book" />
        </div>
      {/if}
    {/if}

  </div>
</main>
