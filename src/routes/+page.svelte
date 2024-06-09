<script lang="ts">
	import { supabase } from "$lib/supabase";
	import { onMount } from "svelte";
    //@ts-ignore
    import showdown from "showdown";
    var converter = new showdown.Converter()
    
    var question = ""


    var messagesHistory: Array<any> = []
    onMount(async () => {
        //@ts-ignore
        messagesHistory = (await supabase.from("messages").select()).data[0].content
        setTimeout(() => {
            scrollToBottom()
        }, 1);
    })

    function textAreaResize() {
        let ask = document.getElementById("ask")
        //@ts-ignore
        ask.style.height = "auto"
        //@ts-ignore
        ask.style.height = (ask.scrollHeight < 200 ? ask.scrollHeight : 200) + "px";
        scrollToBottom()
    }

    async function sendMessage() {
        messagesHistory = [...messagesHistory, {role: "user", content: question}]
        setTimeout(() => {
            scrollToBottom()
        }, 1);
        const { data, error } = await supabase.functions.invoke('ask', {"body": {"question": question}})
        if(!error) {
            
            messagesHistory = [...messagesHistory, {role: "assistant", content: data}]
            question = ""
            textAreaResize()
        }
        else {
            console.error(error)
        }
        setTimeout(() => {
            scrollToBottom()
        }, 1);
    }

    function scrollToBottom() {
        window.scrollTo({
        top: document.body.scrollHeight,
        behavior: 'smooth' // This enables smooth scrolling
        });
    }

    
</script>

<div id="wrapper" class="gap-5 flex flex-col pb-24">
    {#each messagesHistory as message}
        {#if message.role == "user"}
            <div class="bg-gray-300 p-5">
                <h3 class="h3">User :</h3>
                <p>{@html converter.makeHtml(message.content)}</p>
            </div>
            
        {:else}
        <div class="p-5">
            <h3 class="h3">Gougoule AI :</h3>
            <p>{@html converter.makeHtml(message.content)}</p>
        </div>
        {/if}
    {/each}
    <div class="w-[calc(100%-theme(space.6))] mb-5 flex flex-row gap-2 align-middle justify-center fixed bottom-1">
        <textarea bind:value={question} name="ask" class="textarea w-[calc(100%-theme(space.16))]" id="ask" on:input={textAreaResize}></textarea>
        <button on:click={sendMessage} class="w-8">
            <svg data-slot="icon" aria-hidden="true" fill="none" stroke-width="2.5" stroke="black" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <path d="M6 12 3.269 3.125A59.769 59.769 0 0 1 21.485 12 59.768 59.768 0 0 1 3.27 20.875L5.999 12Zm0 0h7.5" stroke-linecap="round" stroke-linejoin="round"></path>
            </svg>
        </button>
    </div>
    
</div>

