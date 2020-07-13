<script>
  import Nav from "./Nav.svelte";
  import * as animateScroll from "svelte-scrollto";
  import { slide, fade, scale } from "svelte/transition";
  import FakeLoading from "./FakeLoading.svelte";
  import Search from "./Search.svelte";
  import ListItem from "./ListItem.svelte";
  let listItems = [];
  let scrollY = 0;
  let displayScroll = false;
  let open = false;
  let loadedMore = false;
  let search = "";
  let loading = false;
  let emptyResults = false;
  let gsroffset = 0;
  $: getResults(search);
  function getResults(search) {
    if (search) {
      if (!loadedMore) {
        emptyResults = false;
        loading = true;
        listItems = [];
        fetch(
          `https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&prop=pageimages|extracts&exintro&explaintext&exsentences=1&exlimit=max&gsrsearch=${search}&origin=*&pithumbsize=400`
        )
          .then(blob => blob.json())
          .then(e => {
            loading = false;
            if (!e.query) {
              emptyResults = true;
            } else {
              emptyResults = false;
              gsroffset = e.continue ? e.continue.gsroffset : 0;
              listItems = Object.values(e.query.pages);
            }
          });
      } else {
        fetch(
          `https://en.wikipedia.org/w/api.php?format=json&action=query&generator=search&gsrnamespace=0&prop=pageimages|extracts&exintro&explaintext&exsentences=1&exlimit=max&gsrsearch=${search}&origin=*&pithumbsize=400&gsroffset=${gsroffset}`
        )
          .then(blob => blob.json())
          .then(e => {
            gsroffset = e.continue ? e.continue.gsroffset : 0;
            listItems = [...listItems, ...Object.values(e.query.pages)];
            loadedMore = false;
          });
      }
    } else {
      listItems = [];
    }
  }
  $: scrollY >= 236 ? (displayScroll = true) : (displayScroll = false);
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
  .book,
  .error {
    font-size: 2.1rem;
    color: #fff;

    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .book img,
  .sad {
    width: 10rem;
  }
  .book p,
  .error p {
    margin-top: -10rem;
  }
  .loading-icon {
    width: 4.9rem;
    margin: 4rem auto 4rem auto;
  }
  .scrollup {
    bottom: 0;
    right: 0;
    background-color: rgba(255, 255, 255, 0.952);
    width: 7rem;
    height: 7rem;
    border-radius: 50%;
    margin: 2rem;
    cursor: pointer;
  }
  .scrollup img {
    width: 80%;
    margin-left: -0.25rem;
  }
  .list-item-container {
    position: relative;
  }
  .list-item-container:hover {
    transform: scale(1.025);
    z-index: 11;
  }
</style>

<svelte:window bind:scrollY />
{#if displayScroll}
  <div
    class="scrollup fixed flex items-center justify-center"
    on:click={() => animateScroll.scrollToTop()}
    transition:fade>
    <img src="./images/arrow-up.svg" alt="" />
  </div>
{/if}
<main>
  <Nav />
  <div class="container">
    <Search
      on:search={e => (search = e.detail)}
      on:open={e => (open = e.detail)}
      {loadedMore}
      on:clear={() => {
        search = '';
        listItems = [];
      }} />
    {#if open}
      {#if loading}
        <div class="list-items">
          <FakeLoading />
        </div>
      {:else if listItems.length && search}
        <div class="list-items">
          {#each listItems as item (item.index)}
            <div class="list-item-container relative" transition:slide|local>
              <ListItem
                img={item.thumbnail ? item.thumbnail.source : ''}
                title={item.title}
                description={item.extract} />
            </div>
          {/each}
          {#if gsroffset !== 400}
            <div class="text-center">
              {#if loadedMore}
                <img
                  src="./images/loading.svg"
                  alt=""
                  class="flex items-center justify-center loading-icon mx-auto" />
              {:else}
                <button
                  class="load-more mx-auto"
                  on:click={() => {
                    loadedMore = true;
                    getResults(search);
                  }}>
                  Load More
                </button>
              {/if}
            </div>
          {/if}
        </div>
      {:else if !emptyResults}
        <div class="book" transition:scale={{ duration: 300 }}>
          <p>Search for Articles</p>
          <img src="./images/book.svg" alt="" class="book" />
        </div>
      {/if}
    {/if}
    {#if emptyResults}
      <div class="error" transition:scale={{ duration: 300 }}>
        <p>No Articles were found</p>
        <img src="./images/sad.svg" alt="" class="sad mx-auto my-8" />
      </div>
    {/if}

  </div>
</main>
