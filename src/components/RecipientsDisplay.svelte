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
            if ( recipientDisplayList.length > 1 && recipentsWidthSum > innerWidth ) {
                recipientDisplayList.pop()
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
                { recipient !== '...' && recipients.length > 1  ? ',' : ''}
                { recipientDisplayList.length === index+1 && lengths &&recipients.length > 1  ? '...' : ''}
            </span>
        {/each}
    </div>
    <div class="badgeContainer">
        {#if lengths }
            <span class="badge">{`+${lengths}`}</span>
        {/if}
    </div>
</main>
<style>
    .main {
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
    .badgeContainer{
        width: 10%;
        margin-top: 6px;
    }
    .recipients {
        display: inline-block;
        overflow: hidden;
        text-overflow: ellipsis;
    }


    @media screen and (max-width: 1440px) {
        .recipients {
            width: 80%;
            overflow: hidden;
        }

        .badgeContainer {
            width: 20%;
        }
    }

    .emailContainer {
        display: inline-block;
        font-size: 16px;
        overflow: hidden;
        color: #333333;
        padding: 5px 10px;
        width: 100%;

    }
    .badge {
        background: #666666;
        height: 100%;
        font-size: 16px;
        padding: 2px 5px 2px 5px;
        color: #f0f0f0;
        border-radius: 3px;
    }

</style>