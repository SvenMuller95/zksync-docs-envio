<script setup lang="ts">
import type { ParsedContent } from '@nuxt/content/dist/runtime/types';

const { seo } = useAppConfig();

const { data: navigation } = await useAsyncData('navigation', () => fetchContentNavigation());
provide('navigation', navigation);

const { data: files } = useLazyFetch<ParsedContent[]>('/api/search.json', {
  default: () => [],
  server: false,
});

useHead({
  meta: [{ name: 'viewport', content: 'width=device-width, initial-scale=1' }],
  link: [{ rel: 'icon', href: '/favicon.ico' }],
  htmlAttrs: {
    lang: 'en',
  },
});

useSeoMeta({
  titleTemplate: `%s - ${seo?.siteName}`,
  ogSiteName: seo?.siteName,
  ogUrl: 'https://docs.zksync.io/',
  ogImageAlt: 'Hyperscaling Ethereum with ZK tech.',
  ogDescription:
    'zkSync Docs bring you all information you need about our protocol, APIs, SDKs, ZK Stack, and ZK Chains. Start with our guides and tutorials, or go deep into our architecture and protocol specification.',
  description:
    'zkSync Docs bring you all information you need about our protocol, APIs, SDKs, ZK Stack, and ZK Chains. Start with our guides and tutorials, or go deep into our architecture and protocol specification.',
  twitterDescription:
    'zkSync Docs bring you all information you need about our protocol, APIs, SDKs, ZK Stack, and ZK Chains. Start with our guides and tutorials, or go deep into our architecture and protocol specification.',
  twitterTitle: `%s`,
  twitterCard: 'summary_large_image',
  twitterSite: '@zksync',
  twitterCreator: '@zkSyncDevs',
  twitterImageAlt: 'Hyperscaling Ethereum with ZK tech.',
});

defineOgImage({
  component: 'OgImageZK',
  title: seo?.siteName,
  description: 'Access detailed guides, references and resources that will help you build with zkSync Era.',
});
</script>

<template>
  <div>
    <NuxtLoadingIndicator />

    <HeaderComponent :search="true" />

    <UMain>
      <NuxtLayout>
        <NuxtPage />
      </NuxtLayout>
    </UMain>

    <FooterComponent />

    <ClientOnly>
      <LazyUContentSearch
        :files="files"
        :navigation="navigation || []"
      />
    </ClientOnly>

    <UNotifications />
  </div>
</template>
