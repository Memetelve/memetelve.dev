<script lang="ts">
  import ListItem from '$components/lists/project-item.svelte';
  import { onMount } from 'svelte';
  import type { PageData } from './$types';
  import { navOptions, pageHeading } from '$stores/navigation';
  import Icon from '$components/icon.svelte';
  import { page } from '$app/stores';
  import { PAGINATION_POSTS_PER_PAGE } from '$lib/consts';
  import Hoverable from '$components/hoverable.svelte';
  import EmptyContent from '$components/empty-content.svelte';

  let curPage = 1,
    totalPages = 1;

  onMount(() => {
    pageHeading.set(`All work | Page ${curPage}`);
    navOptions.set({ down: '', up: '/work' });
  });

  export let data: PageData;

  $: projects?.meta &&
    (totalPages = Math.ceil(projects.meta.total / PAGINATION_POSTS_PER_PAGE));
  $: $page.params, (curPage = parseInt($page.params?.page));
  $: ({ projects } = data);
</script>

<svelte:head>
  <title>kio.dev | work | all work</title>
</svelte:head>

<div class="mt-4 mb-2 flex w-full flex-row items-center justify-between">
  <div class="flex flex-row items-center justify-start gap-2">
    <Icon icon={'List'} />
    <h3 class="font-code text-lg">Page {curPage} of {totalPages}</h3>
  </div>
  <div class="flex flex-row items-center justify-start gap-4">
    <Hoverable>
      <a href="/work/{curPage > 1 ? curPage - 1 : 1}">
        <Icon icon={'ArrowLeft'} />
      </a>
    </Hoverable>
    <Hoverable>
      <a href="/work/{curPage < totalPages ? curPage + 1 : totalPages}">
        <Icon icon={'ArrowRight'} />
      </a>
    </Hoverable>
  </div>
</div>

{#if projects?.data?.length}
  {#each projects.data as project}
    <ListItem {project} />
  {/each}
{:else}
  <EmptyContent />
{/if}
