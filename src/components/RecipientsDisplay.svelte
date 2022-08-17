<script lang="ts">
    import { tick, onMount } from 'svelte';
    export let recipients
    let recipientDisplayList = [];
    let innerWidth = 0;
    let recipientsWidth = 0;
    let recipentsWidthSum = 0;
    let lengths = 0;
    onMount( async () => {
        for ( let recipient of recipients ) {
            recipientDisplayList = [ ...recipientDisplayList, recipient ]
            await tick()
            recipentsWidthSum += recipientsWidth
            if ( recipentsWidthSum > innerWidth ) {
                recipientDisplayList.pop() && recipientDisplayList.push( '...' )
                lengths = (recipients.length - recipientDisplayList.length) + 1;
                recipientDisplayList = recipientDisplayList
                break;
            }
        }
    })
</script>
    <main class="main">
        <div class="emailContainer" bind:clientWidth={innerWidth}>
            {#each recipientDisplayList as recipient, index}
            <span bind:clientWidth={recipientsWidth} class="recipients">
                {recipient}
                { recipient !== '...' && recipients.length > 1 ? ',' : ''}
            </span>
            {/each}
        </div>
        {#if lengths }
            <span class="badge">{`+${lengths}`}</span>
        {/if}
    </main>
<style>
    .main {
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
    .recipients {
        display: inline-block;
        overflow: hidden;
        text-overflow: ellipsis;
    }
    @media screen and (max-width: 1080px) {
        .recipients {
            width: 100%;
        }
    }
    .emailContainer {
        display: inline-block;
        overflow: hidden;
        text-overflow: ellipsis;
        width: 90%;

    }
    .badge {
        background: rgba(3, 3, 3, 0.99);
        height: 100%;
        padding: 4px;
        color: white;
        border-radius: 4px;
    }

</style>