<script>
  let openedNav = false;
  function openNav() {
    if (!openedNav) {
      openedNav = true;
    } else {
      openedNav = false;
    }
  }
</script>

<style>
  .menu img {
    width: 4rem;
  }
  .logo {
    font-size: 2.6rem;
  }
  nav {
    padding: 0rem 2rem;
    box-shadow: 0px 10px 87px -17px rgba(0, 0, 0, 0.75);
    transition: all 0.3s;
    height: 7vh;
    align-items: center;
    z-index: 20;
    background: #fff;
  }
  .opened-nav {
    height: 93vh;
    margin-top: 7vh;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background: #fff;
    transition: all 0.3s;
    transform-origin: top;
    z-index: 19;
    padding: 2rem 0;
  }
  a {
    color: #3b6978;
  }
  p {
    font-size: 2.2rem;
  }
  .contain {
    padding: 0rem 2rem;
  }
  li {
    font-size: 2.1rem;
  }
  .follow {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    margin: 2rem auto;
    text-align: center;
  }
</style>

<div class="opened-nav" style="transform: scaleY({openedNav ? 1 : 0})">
  <div class="contain">
    <p class="text-center">
      A Wikipedia Searcher this
      <a
        href="https://www.freecodecamp.org/learn/coding-interview-prep/take-home-projects/build-a-wikipedia-viewer"
        target="_blank">
        FreeCodeCamp Challenge
      </a>
    </p>
    <ul class="mt-8">
      <li>
        <p>Source Code:</p>
        <ul class="square">
          <li>
            <a
              href="https://github.com/Noureldineadel65/Pascal"
              class="flex items-center"
              target="_blank">
              GitHub
            </a>
          </li>

        </ul>
      </li>
      <li>
        <p>Built with:</p>
        <ul class="square">
          <li>
            <a href="https://svelte.dev/" target="_blank">SvelteJS</a>
          </li>
          <li>
            <a href="https://tailwindcss.com/" target="_blank">TailwindCSS</a>
          </li>
          <li>
            <a
              href="https://www.mediawiki.org/wiki/API:Main_page"
              target="_blank">
              Mediawiki API
            </a>
          </li>
        </ul>
      </li>
      <li class="follow">
        <a href="https://github.com/Noureldineadel65" target="_blank">
          Follow me on GitHub
        </a>
      </li>
    </ul>
  </div>
</div>
<nav class="flex justify-between text-black">
  <div class="logo font-bold">WikiNour</div>
  <div class="menu" on:click={openNav}>
    {#if openedNav}
      <img src="images/close.svg" />
    {:else}
      <img src="images/menu.svg" />
    {/if}
  </div>

</nav>
