<script>
	import { onMount } from 'svelte';
	import '../app.css';
	import { getCookie } from '$lib/modules/cookies';
	import { goto } from '$app/navigation';
	import { supabase } from '$lib/supabase';
	import { Toast, initializeStores } from '@skeletonlabs/skeleton';

	initializeStores();

	// try to log in
	onMount(async () => {
		if(window.location.pathname != "/login") {
			
			let {data, error} = await supabase.auth.getUser()
			if(error) {
				goto("/login")
			}
			
		}
	})
</script>
<Toast />

<slot></slot>

<style></style>
