<script lang="ts">
  import DocumentRoute from '$components/document/route.svelte';
  import type { PageData } from './$types';

  export let data: PageData;

  $: ({ post, headings, routeFetch } = data);
</script>

<DocumentRoute model="post" data={post?.data} {headings} {routeFetch} />
