<script lang="ts">
	import Chats from './Chats.svelte';
	import User from './User.svelte';
	let selectedUsers: string[] = [];
</script>

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

