<script lang="ts">
  import ContentWrapper from '$components/layouts/content-wrapper.svelte';
  import Content from '$components/document/content/content.svelte';
  import { page } from '$app/stores';
  import { onMount } from 'svelte';
  import { navOptions, pageHeading } from '$stores/navigation';
  import { attemptScroll } from '$helpers/scrollTo';
  import type { PostDocument, ProjectDocument, RouteFetch } from '$types';
  import { t } from '$i18n';
  import type { Heading } from '$helpers/pt';

  export let model: 'post' | 'project',
    data: ProjectDocument | PostDocument | undefined,
    headings: Heading[],
    routeFetch: RouteFetch | undefined = undefined;

  const isPost = model === 'post',
    allTags =
      (((tags: PostDocument['tags'] | ProjectDocument['tags'] | undefined) => {
        if (!tags) {
          return undefined;
        }
        return tags.reduce((acc, tag) => {
          tag.title && acc.push(tag.title.toLowerCase());
          return acc;
        }, [] as string[]);
      })(data?.tags)?.join(', ') as string) + ', ' || '';

  onMount(() => {
    navOptions.set({ down: '', up: isPost ? '/blog' : '/work' });
    pageHeading.set(pageName);

    attemptScroll($page);
  });

  $: $page && attemptScroll($page);
  $: pageName = `${isPost ? $t('Thoughts') : $t('My work')}${
    data?.title ? ` | ${data.title}` : ''
  }`;
  $: pageTitle = `kio.dev${data?.title ? ` | ${data.title}` : ''}`;
  $: pageDescription = data?.desc
    ? data.desc
    : $t(`A ${isPost ? 'post' : 'project'} on kio.dev`);
</script>

<svelte:head>
  <title>{pageTitle}</title>
  <meta name="description" content={pageDescription} />
  <meta
    name="keywords"
    content="{allTags}blog, {isPost
      ? 'post'
      : 'project'}, kio.dev, kio, kiosion"
  />
  <meta name="author" content="Kio" />
  <meta property="og:type" content="website" />
  <meta property="og:url" content={$page.url.href} />
  <meta property="og:title" content={pageTitle} />
  <meta
    property="og:description"
    content={data?.desc ? data.desc : $t('A post on kio.dev')}
  />
  <meta property="twitter:url" content={$page.url.href} />
  <meta property="twitter:title" content={pageTitle} />
  <meta property="twitter:description" content={pageDescription} />
  <slot name="meta" />
</svelte:head>

<div data-test-route={isPost ? 'blog' : 'work'}>
  {#if data}
    <ContentWrapper>
      <Content {model} {data} {headings} {routeFetch} />
    </ContentWrapper>
  {/if}
</div>

<style lang="scss">
  div {
    @apply flex h-fit flex-row items-stretch justify-start;
  }
</style>
