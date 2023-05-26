<script lang="ts">
	// The ordering of these imports is critical to your app working properly
	// import '@skeletonlabs/skeleton/themes/theme-sahara.css';
	import '$lib/assets/theme.postcss';
	// If you have source.organizeImports set to true in VSCode, then it will auto change this ordering
	import '@skeletonlabs/skeleton/styles/skeleton.css';
	// Most of your app wide CSS should be put in this file
	import '../app.postcss';
	import Navbar from '$lib/components/Navbar.svelte';
	import NavbarDrawer from '$lib/components/Drawer.Navbar.svelte';
	import { Drawer, drawerStore } from '@skeletonlabs/skeleton';

	import { computePosition, autoUpdate, offset, shift, flip, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';
	storePopup.set({ computePosition, autoUpdate, offset, shift, flip, arrow });

	import { FirebaseApp } from 'sveltefire';
	import { auth, firestore } from '$lib/firebase';
</script>

<FirebaseApp {auth} {firestore}>
	<Drawer>
		{#if $drawerStore.id === 'navbar'}
			<NavbarDrawer />
		{:else}
			<!-- (fallback contents) -->
		{/if}
	</Drawer>
	<Navbar />
	<slot />
</FirebaseApp>
