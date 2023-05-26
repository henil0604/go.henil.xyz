<script>
	import { AppBar, Avatar, drawerStore } from '@skeletonlabs/skeleton';
	import Icon from '@iconify/svelte';
	import { APP_NAME, NavbarActions } from '$lib/const';
	import Tooltip from '$lib/components/Tooltip.svelte';
	import { page } from '$app/stores';
	import { User } from 'sveltefire';
	import { signOut } from 'firebase/auth';
	import { auth } from '$lib/firebase';

	function showMenu() {
		drawerStore.open({
			id: 'navbar',
			width: 'max-md:w-full w-[500px]',
			padding: 'p-4',
			rounded: 'rounded-xl'
		});
	}
</script>

<AppBar class="shadow-lg backdrop-blur-xl variant-ghost-surface">
	<svelte:fragment slot="lead">
		<div class="h3 font-semibold">{APP_NAME}</div>
	</svelte:fragment>
	<svelte:fragment slot="trail">
		<!-- FOR PC -->
		<div class="max-md:hidden">
			<div class="flex gap-2">
				{#each NavbarActions as action}
					{#if action.href !== $page.url.pathname}
						<Tooltip text={action.name} variant={action.variant}>
							<a href={action.href} class="btn-icon btn-icon-sm variant-{action.variant}"
								><Icon class="text-xl" icon={action.icon} /></a
							>
						</Tooltip>
					{/if}
				{/each}
				<User let:user>
					{@const initials =
						user.displayName
							?.split(' ')
							.map((e) => e.charAt(0).toUpperCase())
							.join('') || ''}
					<Tooltip event="click" variant="soft" text="Hey, {user.displayName}">
						<svelte:fragment slot="tooltip-content">
							<button
								on:click={() => signOut(auth)}
								class="btn w-full mt-3 btn-sm variant-filled-error">Logout</button
							>
						</svelte:fragment>
						<Avatar width="w-8" src={user.photoURL || ''} {initials} />
					</Tooltip>
				</User>
			</div>
		</div>
		<!-- FOR MOBILE -->
		<div class="hidden max-md:block">
			<Tooltip text="Menu">
				<button class="btn-icon variant-filled-primary" on:click={showMenu}
					><Icon class="text-2xl" icon="mdi:menu" /></button
				>
			</Tooltip>
		</div>
	</svelte:fragment>
</AppBar>
