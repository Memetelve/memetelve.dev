<script lang="ts">
  import { navigating } from '$app/stores';
  import { onMount, onDestroy } from 'svelte';
  import { isDesktop } from '$helpers/responsive';
  import type { Unsubscriber } from 'svelte/store';

  export let element: HTMLDivElement;

  let unsubscribe: Unsubscriber;

  onMount(() => {
    unsubscribe ||= navigating.subscribe((state) => {
      if (state !== null) {
        return;
      }
      element.scrollTo({
        top: 0,
        behavior: 'smooth'
      });
    });
  });

  onDestroy(() => unsubscribe?.());
</script>

<div class:isDesktop={$isDesktop} tabindex="-1" bind:this={element}>
  <slot />
</div>

<style lang="scss">
  div {
    @apply relative h-full w-full overflow-visible overflow-x-clip overflow-y-scroll bg-stone-100 p-8  duration-150;

    &:not(.isDesktop) {
      @apply rounded-t-2xl;
    }
  }

  :global(.dark) {
    div {
      @apply bg-stone-900;
    }
  }
</style>
