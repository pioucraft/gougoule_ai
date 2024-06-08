<script>
	import { supabase } from "$lib/supabase";
	import { onMount } from "svelte";

    onMount(() => {
        console.log(supabase.functions.invoke("ask"))
    })
    
</script>