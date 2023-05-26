<script>
	import { page } from '$app/stores';
	import { APP_NAME, NavbarActions } from '$lib/const';
	import Icon from '@iconify/svelte';
	import { drawerStore } from '@skeletonlabs/skeleton';
	import Tooltip from '$lib/components/Tooltip.svelte';
	import { User } from 'sveltefire';
	import LoginWithGoogleButton from '$lib/components/LoginWithGoogleButton.svelte';
	import { signOut } from 'firebase/auth';
	import { auth } from '$lib/firebase';
</script>

<div class="flex w-full h-full flex-col justify-between">
	<!-- Header -->
	<div class="h-fit flex justify-between items-center p-2 px-4">
		<!-- title -->
		<div class="h1 font-semibold">
			{APP_NAME}
		</div>

		<!-- actions -->
		<div class="flex gap-2">
			<button class="btn-icon">
				<Tooltip text="Close" placement="left">
					<button on:click={drawerStore.close} class="btn-icon variant-filled-primary"
						><Icon class="text-2xl" icon="mdi:close" /></button
					>
				</Tooltip>
			</button>
		</div>
	</div>
	<hr />
	<!-- content -->
	<div class="h-full flex flex-col">
		{#each NavbarActions as action}
			<a
				href={action.href}
				class="p-4 flex items-center gap-2 border-y border-surface-700 {action.href ===
				$page.url.pathname
					? 'variant-filled-primary'
					: 'hover:variant-soft-surface'}"
			>
				<Icon class="text-2xl" icon={action.icon} />
				<div class="text-lg">
					{action.name}
				</div>
			</a>
		{/each}
	</div>

	<!-- Footer -->
	<div class="h-fit p-2">
		<User let:user>
			<div class="p-3 flex items-center justify-between">
				<div class="font-semibold">
					Hey, <span class="text-secondary-500">{user.displayName}</span>
				</div>
				<button on:click={() => signOut(auth)} class="btn btn-sm gap-2 variant-filled-error"
					><Icon class="text-2xl" icon="mdi:logout" />Logout</button
				>
			</div>
			<svelte:fragment slot="signedOut">
				<LoginWithGoogleButton classes="w-full" />
			</svelte:fragment>
		</User>
	</div>
</div>
