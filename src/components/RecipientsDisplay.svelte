<script lang="ts">
    import { tick, onMount } from 'svelte';
    export let recipients
    let recipientDisplayList = [];
    let innerWidth = 0;//Inner width is the width of email container width.
    let recipientsWidth = 0; //recipientsWidth width is the width of email length.
    let recipentsWidthSum = 0; // recipientsWidthSum is the total width of recipientsWidth
    let lengths = 0;
    onMount( async () => {
        for ( let recipient of recipients ) {
            //Creating recipientDisplayList to store each recipient's emails
            recipientDisplayList = [ ...recipientDisplayList, recipient ]
            //The tick function is unlike other lifecycle functions in that you can call it any time
            await tick()
            //recipientsWidth is the width of the individual email.
            recipentsWidthSum += (recipientsWidth + 40)
            // recipentsWidthSum will get total width and added 40 to make ellipsis this fit perfectly with rest of text
            if ( recipientDisplayList.length > 1 && recipentsWidthSum > innerWidth ) {
                //if recipientsWidthSum is greater than innerWidth and if recipientDisplayList contains arrayList more than one
                //then it will start removing recipientDisplayList items.
                //if list is not fitting then it will check  recipientDisplayList length and if recipientDisplayList length is less than one
                //then it will not pop.
                recipientDisplayList.pop()
                lengths = (recipients.length - recipientDisplayList.length)
                //I am assigning length cause in View I have to show badge which I can show base on length.
                recipientDisplayList = recipientDisplayList
                //I am reassigning the recipientDisplayList with new recipientDisplayList.
                break;
            }
        }
    } )
</script>
<main class="main">
    <!--Here i am getting innerWidth so this is container which shows email container width -->
    <div class="email-container" bind:clientWidth={innerWidth}>
        {#each recipientDisplayList as recipient, index}
            <!-- Here i am getting recipientsWidth so this is width, which shows the individual emails width  -->
            <span bind:clientWidth={recipientsWidth} class="recipients">
                {recipient}
                <!-- I am showing comma if there is no ... because in design there is no comma after .... -->
                { recipient !== '...' && recipients.length > 1 && recipientDisplayList.length !== index + 1 ? ',' : ''}
                <!--this handle the showing of ellipsis base on the this condition if recipientDisplayList lenght is equal to the last index and if the -->
                <!-- recipients.length is greater than one then we showing ellipsis-->
                { recipientDisplayList.length === index + 1 && lengths && recipients.length > 1 ? '...' : ''}
            </span>
        {/each}
    </div>
    <div class="badge-container">
        <!-- This show the badge base on the remaining emails, if email is remaining emails is zero then it will not show the badge -->
        <!-- but if remaining email is more than one then it will show the badge-->
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
    .badge-container {
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
        .badge-container {
            width: 20%;
        }
    }
    .email-container {
        display: inline-block;
        font-size: 16px;
        overflow: hidden;
        color: #333333;
        padding: 5px 10px;
        width: 100%;
    }
    .badge {
        background-color: #666666;
        height: 100%;
        font-size: 16px;
        padding: 2px 5px;
        color: #f0f0f0;
        border-radius: 3px;
    }
</style>