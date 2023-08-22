<script lang="ts">
	import { text } from "@sveltejs/kit";

  let messages: any[] = [];
  let userInputRight = '';
  let userInputLeft = '';


</script>
<div class="mess">
    <p>
        {#each messages as message (message.text)}
          <div class="message {message.talker}">
            <span class="talker">{message.talker}</span>
            {message.text}
          </div>
        {/each}
    </p>
        <div class="inputs leftInput">
            <input type="text" placeholder="Type a message.." bind:value={userInputLeft} on:keydown={(s) => {
                if(s.key === 'Enter'){
                    for (let i = 0; i < messages.length; i++) {
                        if (messages[i].text == userInputLeft) {
                            userInputLeft = userInputLeft + " ";
                        }  
                    }

                    if (userInputLeft !== "") {
                    messages = [...messages, { text: userInputLeft, talker: 'userLeft' }];
                    userInputLeft = '';
                    }
                }
            }}/>
        </div>

        <div class="inputs rightInput">
            <input type="text" placeholder="Type a message.." bind:value={userInputRight} on:keydown={(e) => {
                if(e.key === 'Enter'){
                    for (let i = 0; i < messages.length; i++) {
                            if (messages[i].text == userInputRight) {
                                userInputRight = userInputRight + " ";
                            } 
                    }
                    if (userInputRight !== '') {
                       
                    messages = [...messages, { text: userInputRight, talker: 'userRight' }];
                    userInputRight = '';
                    }   
                }
            }}/>
        </div>
</div>

<style>

    .mess{
       
        display: grid;
        grid-column: 2;
        gap: 10px;
        grid-template-columns: 1fr 1fr;
        grid-template-rows: auto 40px;
        height: 90vh;    
        overflow: auto;
        background-color: rgb(116, 96, 134);

    }

    .message {

        padding: 8px;
        margin: 8px;
        border-radius: 8px;
    
    }

    .userLeft {

        display: block;
        background-color: lightblue;
        grid-column-start: 1;

    }

    .userRight {

        display: block;
        background-color: lightgreen;
        grid-column-start: 2;

    }

    .talker {

    font-weight: bold;

    }
    .rightInput{
        
        position: fixed;
        bottom: 86px;
        left: 1600px;
        width: 100%;
        padding: 10px;
    }
    

    .leftInput{
        position: fixed;
        bottom: 86px;

        width: 100%;
        padding: 10px;

    }

    .inputs {

        background-color: rgb(42, 46, 41);
        grid-row: 2;

    }

</style>
