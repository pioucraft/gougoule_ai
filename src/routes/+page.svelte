<script>
	import { supabase } from "$lib/supabase";
	import { onMount } from "svelte";

    onMount(async () => {
        const { data, error } = await supabase.functions.invoke('ask', {
            body: { foo: 'bar' }
        })
        console.log(data, error)
    })
    
</script>