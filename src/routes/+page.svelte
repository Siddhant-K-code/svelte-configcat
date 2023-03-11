<script>
	import * as configcat from 'configcat-js';
	import { onMount } from "svelte";
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';

	/**
   * @type {boolean}
   */
	 export let featureFlagStatus;

	onMount(async () => {
    // Connect to ConfigCat client
    const configCatClient = configcat.getClient(
      "ACLbCEY2OEOIFbi28r8qNg/sAaw80cboUGdo3E_Ep-CzA"
    );

    featureFlagStatus = await configCatClient.getValueAsync("newHeaderFlag", true);
  });
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>
	<!-- Show fact only if flag is toggled on -->
	{#if featureFlagStatus}
    <p>Feature Flag is on 🍊</p>
  {/if}

  <!-- Show message if flag is toggled off -->
  {#if !featureFlagStatus}
    <p>Ops! This feature is currently unavailable</p>
  {/if}

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
