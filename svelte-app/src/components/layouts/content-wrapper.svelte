<script lang="ts">
  export let classes = '',
    fixed = false;
</script>

{#if fixed}
  <div class="w-full max-w-[42rem] pb-20">
    <slot />
  </div>
{:else}
  <div
    class="mx-auto mb-8 w-full max-w-[28rem] px-4 pb-12 transition-[transform] md:w-[32rem] md:max-w-none md:px-0 lg:w-[38rem] xl:w-[46rem] 2xl:w-[52rem] {classes}"
  >
    <slot />
  </div>
{/if}
