<script>
	import LoadingOverlay from '$lib/components/LoadingOverlay.svelte';
	import LoginWithGoogleButton from '$lib/components/LoginWithGoogleButton.svelte';
	import Tooltip from '$lib/components/Tooltip.svelte';
	import { RadioGroup, RadioItem } from '@skeletonlabs/skeleton';
	import { useForm, Hint, validators, minLength, required } from 'svelte-use-form';
	import { User } from 'sveltefire';

	const form = useForm({
		url: {
			initial: 'https://google.com'
		},
		visibility: {
			initial: 'unlisted',
			validators: [required]
		},
		password: {
			initial: '',
			validators: [minLength(3), required]
		}
	});

	let loading = false;

	async function handleSubmit() {
		loading = true;

		loading = false;
	}
</script>

<div class="w-full h-full flex justify-center pt-4">
	<div
		class="card relative min-w-[400px] max-md:min-w-[calc(100%-10px)] w-fit h-fit p-3 shadow-lg transition flex flex-col"
	>
		<User>
			{#if loading}
				<LoadingOverlay />
			{/if}

			<div class="h3 font-semibold">Create new link</div>
			<hr class="mt-1 mb-4" />

			<form class="flex flex-col gap-2" use:form on:submit|preventDefault={handleSubmit}>
				<label class="label">
					<span>URL</span>
					<Tooltip text="URL you want to shorten" placement="bottom">
						<input required class="input" name="url" type="url" placeholder="https://google.com" />
					</Tooltip>
				</label>

				<!-- svelte-ignore a11y-label-has-associated-control -->
				<label class="label">
					<RadioGroup class="w-fit">
						<Tooltip text="Will be shown to explore page" placement="bottom">
							<RadioItem bind:group={$form.visibility.value} name="visibility" value={'public'}
								>Public</RadioItem
							>
						</Tooltip>
						<Tooltip text="Will not be shown to explore page" placement="bottom">
							<RadioItem bind:group={$form.visibility.value} name="visibility" value={'unlisted'}
								>Unlisted</RadioItem
							>
						</Tooltip>
						<Tooltip text="can only be accessed with password" placement="bottom">
							<RadioItem bind:group={$form.visibility.value} name="visibility" value={'private'}
								>Private</RadioItem
							>
						</Tooltip>
					</RadioGroup>
				</label>

				{#if $form.visibility?.value === 'private'}
					<label class="label w-full">
						<span>Password</span>
						<input
							class="input max-md:text-sm"
							type="password"
							placeholder="the-secret"
							name="password"
							class:input-error={!$form.password?.valid && $form.password?.touched}
						/>
					</label>
				{/if}

				<div class="w-full flex justify-end">
					<button disabled={!$form.valid} type="submit" class="mt-4 btn variant-ghost-secondary"
						>Create</button
					>
				</div>
			</form>

			<svelte:fragment slot="signedOut">
				<LoginWithGoogleButton />
			</svelte:fragment>
		</User>
	</div>
</div>
