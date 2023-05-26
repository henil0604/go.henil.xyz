<script lang="ts">
	import { auth } from '$lib/firebase';
	import Icon from '@iconify/svelte';
	import { GoogleAuthProvider, signInWithPopup } from 'firebase/auth';

	export let classes = '';

	function handleLogin() {
		const google = new GoogleAuthProvider();
		signInWithPopup(auth, google);
	}
</script>

<button
	on:click={handleLogin}
	class="btn flex gap-2 bg-white text-black font-semibold text-lg {classes}"
>
	<Icon class="text-xl" icon="flat-color-icons:google" />
	Login with Google
</button>
