<script lang="ts">
  import ContentWrapper from '$components/layouts/content-wrapper.svelte';
  import PortableText from '$components/portable-text/portable-text.svelte';
  import { page } from '$app/stores';
  import { setupNavigation } from '$helpers/navigation';
  import { pageHeading } from '$stores/navigation';
  import { onMount, onDestroy } from 'svelte';
  import { t } from '$i18n';
  import Divider from '$components/divider.svelte';
  import IconHeader from '$components/headings/icon-header.svelte';
  import EmptyContent from '$components/empty-content.svelte';
  import type { PageData } from './$types';
  import type { Unsubscriber } from 'svelte/store';

  let subscribers = [] as Unsubscriber[];

  onMount(() => {
    setupNavigation($page?.url?.pathname);

    subscribers = [
      t.subscribe((_) => {
        pageHeading.set(`kio.dev | ${$t('Index')}`);
      })
    ];
  });

  onDestroy(() => {
    subscribers.forEach((unsub) => unsub());
  });

  export let data: PageData;

  $: about = data.about?.data;
  $: pageTitle = `kio.dev | ${$t('Index')}`;
  $: description = $t('A bit about me, my work, and what I do');
</script>

<svelte:head>
  <title>{pageTitle}</title>
  <meta name="description" content={description} />
  <meta name="keywords" content="About, kio.dev, kio, kiosion" />
  <meta name="author" content="Kio" />
  <meta property="og:type" content="website" />
  <meta property="og:url" content={$page.url.href} />
  <meta property="og:title" content={pageTitle} />
  <meta property="og:description" content={description} />
  <meta property="twitter:url" content={$page.url.href} />
  <meta property="twitter:title" content={pageTitle} />
  <meta property="twitter:description" content={description} />
</svelte:head>

<div data-test-route="about">
  <ContentWrapper fixed>
    {#if about}
      <IconHeader icon="User" text={$t('About me')} />
      <div>
        <PortableText text={about.bio} />
      </div>
      {#if about.bio && about.now}
        <Divider />
      {/if}
      {#if about.now}
        <IconHeader icon="Clock" text={$t("What I'm up to now")} />
        <div>
          <PortableText text={about.now} />
        </div>
      {/if}
    {:else}
      <EmptyContent />
    {/if}
  </ContentWrapper>
</div>

<style lang="scss">
  div {
    div {
      @apply mx-1 font-sans text-base text-stone-700;
    }
  }

  :global(.dark) {
    div {
      div {
        @apply text-stone-200;
      }
    }
  }
</style>
