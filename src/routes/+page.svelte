<input bind:value={email} type="text" placeholder="username">
<input bind:value={password} type="password" placeholder="password">
<button on:click={login}>Log in</button>

<script>
    

    var email = ""
    var password = ""

    import { supabase } from "$lib/supabase";

    async function login() {
        const { data, error } = await supabase.auth.signInWithPassword({
            email: email,
            password: password,
        })
        console.log(data, error)
    }
</script>