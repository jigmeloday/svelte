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
            //Creating recipientDisplayList to store each recipient's emails
            recipientDisplayList = [ ...recipientDisplayList, recipient ]
            await tick()
            recipentsWidthSum += (recipientsWidth+40)
            //will get total width and added 40 to make ellipsis  to fit perfectly with rest of text
            if ( recipientDisplayList.length > 1 && recipentsWidthSum > innerWidth ) {
                //if recipientsWidthSum is greater than innerWidth(innerWidth is the width of the email container ) and if recipientDisplayList
                //contains arrayList more than one then it will start removing recipientDisplayList items.
                //First it will check the size(innerWidth and recipentsWidthSum) from this it will assume whether list is fitting or not.
                //if list is not fitting then it will check  recipientDisplayList length and if recipientDisplayList length is less than one
                //then it will not pop because we need to show first email with ellipsis.
                recipientDisplayList.pop()
                lengths = (recipients.length - recipientDisplayList.length)
                //am assigning length cause in View i have to show badge which i can show base on length.
                recipientDisplayList = recipientDisplayList
                // am resigning the recipientDisplayList with new recipientDisplayList.
                break;
            }
        }
    })
</script>
<main class="main">
    <!--        Here i am getting innerWidth so this is container which shows email container width -->
    <div class="emailContainer" bind:clientWidth={innerWidth}>
        {#each recipientDisplayList as recipient, index}
            <!--        Here i am getting recipientsWidth so this is width, which shows the individual emails width  -->
            <span bind:clientWidth={recipientsWidth} class="recipients">
                {recipient}
                <!--                    I am showing comma if there is no ... because in design there is no comma after .... -->
                { recipient !== '...' && recipients.length > 1  ? ',' : ''}
                <!--this handle the showing of ellipsis base on the this condition if recipientDisplayList lenght is equal to the last index and if the -->
                <!--                    recipients.length is greater than one then we showing ellipsis-->
                { recipientDisplayList.length === index+1 && lengths &&recipients.length > 1  ? '...' : ''}
            </span>
        {/each}
    </div>
    <div class="badgeContainer">
        <!--           This show the badge base on the remaining emails, if email is remaining emails is zero then it will not show the badge -->
        <!--           but if remaining email is more than one then it will show the badge-->
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