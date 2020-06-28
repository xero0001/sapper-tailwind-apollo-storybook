<script context="module">
  import { gql } from "apollo-boost";
  import client from "../lib/apollo";

  const BANNERS = gql`
    {
      banners: getIssuedBanners {
        id
        title
      }
    }
  `;

  export async function preload() {
    return {
      cache: await client.query({
        query: BANNERS
      })
    };
  }
</script>

<script>
  import { setClient, getClient, restore, query } from "svelte-apollo";

  export let cache;
  restore(client, BANNERS, cache.data);

  const banners = query(client, { query: BANNERS });
</script>

<svelte:head>
  <title>Sapper project template</title>
</svelte:head>

<h1 class="text-center text-2xl my-4 text-ink-800">Great success!</h1>

<div class="flex flex-col items-center mb-4">
  <img alt="Sapper Logo" class="w-2/3 lg:w-1/3" src="sapper-logo.svg" />
  <img alt="Tailwind Logo" class="w-2/3 lg:w-1/3" src="tailwind-logo.svg" />
</div>

<p class="text-center">
  <strong>
    Try editing this file (src/routes/index.svelte) to test live reloading.
  </strong>
</p>

<ul>
  {#await $banners}
    <li>Loading...</li>
  {:then result}
    {#each result.data.banners as banner (banner.id)}
      <li>{banner.id}</li>
      <li>{banner.title}</li>
    {:else}
      <li>No banners found</li>
    {/each}
  {:catch error}
    <li>Error loading banners: {error}</li>
  {/await}
</ul>
