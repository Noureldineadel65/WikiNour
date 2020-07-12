<script>
  import { scale } from "svelte/transition";

  import ListItem from "./ListItem.svelte";
</script>

<style>
  .list-item-container-show :global(li) {
    min-height: 7rem;
    animation: fade 1s alternate infinite;
  }
  @keyframes fade {
    0% {
      opacity: 0.8;
    }
    100% {
      opacity: 0.6;
    }
  }
</style>

<div class="list-item-container-show" in:scale|local>
  <ListItem />
</div>
<div class="list-item-container-show" in:scale|local>
  <ListItem />
</div>
<div class="list-item-container-show" in:scale|local>
  <ListItem />
</div>
<div class="list-item-container-show" in:scale|local>
  <ListItem />
</div>
<div class="list-item-container-show" in:scale|local>
  <ListItem />
</div>
<div class="list-item-container-show" in:scale|local>
  <ListItem />
</div>
