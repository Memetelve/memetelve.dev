<script lang="ts">
  import NavLink from '$components/nav/nav-link.svelte';
  import SliderIndicator from '$components/nav/slider-indicator.svelte';
  import { navLinks } from '$stores/navigation';

  let navLinksContainer: HTMLElement;
</script>

<div bind:this={navLinksContainer}>
  <ul>
    {#each $navLinks as link, index}
      <NavLink {link} {index} />
    {/each}
    <SliderIndicator container={navLinksContainer} />
  </ul>
</div>

<style lang="scss">
  ul {
    @apply relative flex flex-col items-start pt-5 text-base;
  }
</style>
