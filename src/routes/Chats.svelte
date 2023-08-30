<script lang="ts">
	export let leftUser: string | undefined;
	export let rightUser: string | undefined;
	let messages: { text: string; talker: string; right: boolean }[] = [];
	let userInputRight = '';
	let userInputLeft = '';
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
				<div class="msgContainer">
					<div class="message" class:right={message.right}>
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
		height: 100%;
	}
	.mess p {
		flex-grow: 1;
		display: flex;
		flex-direction: column;
		gap: 5px;
		padding: 15px;
		user-select: none;
	}
	.inputs {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
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
</style>
