<div class="w-screen h-screen flex flex-col justify-center items-center">
    <label class="label">
        <span>New password :</span>
        <div class="input-group grid grid-cols-[auto_3rem]">
            
            <input id="password" type={typeOfPasswordInput} class="input" placeholder="password" on:input={(e) => password = e.currentTarget.value}>
            
            
            <button on:click={() => typeOfPasswordInput = typeOfPasswordInput == "text" ? "password": "text"} class="input-group-shim">
                {#if typeOfPasswordInput == "text"}
                <svg data-slot="icon" aria-hidden="true" fill="none" stroke-width="3.5" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" stroke-linecap="round" stroke-linejoin="round"></path>
                    <path d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" stroke-linecap="round" stroke-linejoin="round"></path>
                </svg>
                {:else}
                <svg data-slot="icon" aria-hidden="true" fill="none" stroke-width="3.5" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path d="M3.98 8.223A10.477 10.477 0 0 0 1.934 12C3.226 16.338 7.244 19.5 12 19.5c.993 0 1.953-.138 2.863-.395M6.228 6.228A10.451 10.451 0 0 1 12 4.5c4.756 0 8.773 3.162 10.065 7.498a10.522 10.522 0 0 1-4.293 5.774M6.228 6.228 3 3m3.228 3.228 3.65 3.65m7.894 7.894L21 21m-3.228-3.228-3.65-3.65m0 0a3 3 0 1 0-4.243-4.243m4.242 4.242L9.88 9.88" stroke-linecap="round" stroke-linejoin="round"></path>
                </svg>
                {/if}
                
            </button>
        </div>
    </label>
    <button class="mt-2 btn variant-filled-primary" on:click={change}>Update my password</button>
</div>

<script lang="ts">
	import { goto } from "$app/navigation";


    import { supabase } from "$lib/supabase";
	import { getToastStore } from '@skeletonlabs/skeleton';
    import type { ToastSettings, ToastStore } from '@skeletonlabs/skeleton';

	const toastStore = getToastStore();
    var password = ""
    var typeOfPasswordInput = "password"

    async function change() {
        const { data, error } = await supabase.auth.updateUser({
            password: password
        })
        if(error) {
            const t: ToastSettings = {
                message: error.message,
                timeout: 3000
            };
            toastStore.trigger(t);
        }
        else {
            const t: ToastSettings = {
                message: "Password updated successfully",
                timeout: 3000
            };
            toastStore.trigger(t);
            goto("/")
        }
    }

</script>