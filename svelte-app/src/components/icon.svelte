<script lang="ts">
  import icons from '$lib/icons';

  export let icon: string,
    hovered = false,
    inline = false,
    width = 20,
    height = width,
    classes = '',
    classNames = '',
    style = '';
</script>

<div class="w-[{width}px] {inline ? 'inline' : ''} {classes} {classNames}">
  {#await icons.get(icon) then svg}
    <svelte:component
      this={svg}
      {width}
      {height}
      class="{hovered ? 'text-gray-600 dark:text-gray-300' : ''} {inline
        ? 'inline'
        : ''}"
      {style}
    />
  {/await}
</div>
