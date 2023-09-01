<script lang="ts">
	import { onMount } from 'svelte';
	import Chats from './Chats.svelte';
	import Logreq from './Logreq.svelte';
	import User from './User.svelte';
	
	
	let selectedUsers: string[] = [];
	export let Authentication: boolean = false;


	onMount(() => {
		let keys = Object.keys(localStorage);
		for (let i = 0; i < keys.length; i++) {
			const key = keys[i];
			if (key.startsWith('currentAccount: ')) {
				Authentication = true;
			}
			else{
				Authentication = false;
			}
		}
	});

</script>
{#if Authentication !== false}
	

	<div>
		<User
			class="userArea"
			on:selected={(e) => {
				selectedUsers = [...selectedUsers, e.detail];
			}}
			on:unselected={(e) => {
				selectedUsers = selectedUsers.filter((user) => user !== e.detail);
			}}
		/>

		<Chats leftUser={selectedUsers[0]} rightUser={selectedUsers[1]} />
	</div>

{:else}
<Logreq />
{/if}

<style>
	:global(.userArea) {
		min-height: 50%;
	}
	:global(*) {
		box-sizing: border-box;
	}
	:global(html, body) {
		margin: 0;
		padding: 0;
		height: 100%;
		width: 100%;
		background-color: rgb(40, 40, 41);
	}
	div {
		display: grid;
		grid-template-columns: 1fr 3fr;
		width: 100%;
		height: 100%;
	}
	@media (max-width: 600px) {
		div {
			display: flex;
			flex-direction: column;
		}
	}
</style>

