<script lang="ts">
	import { text } from "@sveltejs/kit";
	import { onMount } from "svelte";

	export let leftUser: string | undefined;
	export let rightUser: string | undefined;
	let messages: { text: string; talker: string; right: boolean }[] = [];
	let userInputRight = '';
	let userInputLeft = '';

	onMount(() => {
		let p: any = localStorage.getItem("messages");
		let oldMessages = JSON.parse(p);
		for (let i = 0; i < oldMessages.length; i++) {
			let otext = oldMessages[i].text;
			let otalker = oldMessages[i].talker;
			messages = [ ...oldMessages ]
			console.log(messages);
		}
		
	})

	let clickCount = 0;

	

</script>

<div class="mess">
	{#if !leftUser || !rightUser}
		<div
			style="
                display: flex;
                flex-direction: row-reverse;
                justify-content: center;
                text-align:center;
                align-items: center;
                font-weight: bolder;
                font-size: 1.5em;
                height: 100%;"
		>
			<p>Select two users to start chatting</p>
		</div>
	{:else}
		<p>
			{#each messages as message (message.text)}
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<div class="msgContainer" >
					<div class="message" class:right={message.right} on:click={() => {
						clickCount++;
						if (clickCount === 2) {
			
							alert("aa");

							clickCount = 0;

						}
					}} >
						<span class="talker">{message.talker}</span>
						{message.text}
					</div>
				</div>
			{/each}
		</p>
		<div class="inputs">
			<input
				type="text"
				placeholder={rightUser}
				bind:value={userInputRight}
				on:keydown={(e) => {
					if (e.key === 'Enter') {
						for (let i = 0; i < messages.length; i++) {
							if (messages[i].text == userInputRight) {
								userInputRight = userInputRight + ' ';
							}
						}
						if (userInputRight !== '') {
							messages = [
								...messages,
								{ text: userInputRight, talker: rightUser || '', right: false }
							];
							for (let i = 0; i < messages.length; i++) {
								let text = messages[i].text;
								let talker = messages[i].talker;
								let messJSON = JSON.stringify(messages);
								localStorage.setItem("messages", messJSON);
							}
							userInputRight = '';
						}
					}
				}}
			/>
			<input
				type="text"
				placeholder={leftUser}
				bind:value={userInputLeft}
				on:keydown={(s) => {
					if (s.key === 'Enter') {
						for (let i = 0; i < messages.length; i++) {
							if (messages[i].text == userInputLeft) {
								userInputLeft = userInputLeft + ' ';
							}
						}

						if (userInputLeft !== '') {
							messages = [
								...messages,
								{ text: userInputLeft, talker: leftUser || '', right: true }
							];
							for (let i = 0; i < messages.length; i++) {
								let text = messages[i].text;
								let talker = messages[i].talker;
								let messJSON = JSON.stringify(messages);
								localStorage.setItem("messages", messJSON);
							}
							userInputLeft = '';
						}
					}
				}}
			/>
		</div>
	{/if}
</div>

<style>
	.mess {
		display: flex;
		flex-direction: column;
		overflow: auto;
		background-color: #141414;
	}
	.mess p {
		flex-grow: 1;
		display: flex;
		flex-direction: column;
		gap: 5px;
		padding: 15px;
		user-select: none;
		overflow: auto;
	}
	.inputs {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		background-color: #2c2c2c;
		padding: 10px;
	}
	.message {
		background-color: lightblue;
		max-width: fit-content;
		padding: 5px;
		border-radius: 5px;
	}
	.message.right {
		background-color: lightgreen;
		position: relative;
		margin-left: auto;
	}
		/* width */
	::-webkit-scrollbar {
		width: 10px;
	}

	/* Track */
	::-webkit-scrollbar-track {
		box-shadow: inset 0 0 5px rgb(58, 57, 57); 
		border-radius: 10px;
	}
	
	/* Handle */
	::-webkit-scrollbar-thumb {
		background: rgb(48, 45, 45); 
		border-radius: 10px;
	}

	/* Handle on hover */
	::-webkit-scrollbar-thumb:hover {
		background: rgb(37, 36, 36); 
	}
</style>
