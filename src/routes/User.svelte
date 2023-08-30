<script lang="ts">
	import { createEventDispatcher, onMount } from 'svelte';

	let newUser: { name: string; selected: boolean }[] = [];
	let usr: string;

	onMount(() => {
		let keys = Object.keys(localStorage);
		for (let i = 0; i < keys.length; i++) {
			const key = keys[i];
			if (key.startsWith('user: ')) {
				let name = key.replace('user: ', '');
				if (name !== '') {
					newUser = [...newUser, { name, selected: false }];
				}
			}
		}
	});
	const dispatch = createEventDispatcher();
	let cls = $$props.class;
</script>

<div class="allusr {cls}">
	<header>
		<input
			autofocus
			type="text"
			placeholder="New User"
			bind:value={usr}
			on:keydown={(e) => {
				if (e.key === 'Enter') {
					newUser = [...newUser, { name: usr, selected: false }];
					localStorage.setItem(`user: ${usr}`, usr);
					usr = '';
				}
			}}
		/>
	</header>
	<nav>
		{#each newUser as user}
			<div class="usr" class:selected={user.selected}>
				<p
					class="users"
					on:click={(_) => {
						if (user.selected) {
							dispatch('unselected', user.name);
							user.selected = false;
						} else {
							dispatch('selected', user.name);
							user.selected = true;
						}
					}}
				>
					{user.name}
				</p>
				<button
					on:click={() => {
						localStorage.removeItem(`user: ${user.name}`);
						newUser = newUser.filter((u) => u.name !== user.name);
					}}>-</button
				>
			</div>
		{/each}
	</nav>
</div>

<style>
	p {
		margin: 0;
		line-height: 40px;
	}
	.allusr {
		overflow: auto;
		text-align: center;
	}

	header {
		height: 70px;
		border-bottom: 1px solid black;
		display: flex;
		align-items: center;
		align-content: center;
		justify-content: center;
	}
	input {
		width: 70%;
		height: 40px;
		padding: 5px;
	}

	.users {
		color: rgb(209, 203, 203);
		height: 40px;
		user-select: none;
		flex-grow: 1;
		cursor: pointer;
	}

	.selected {
		background-color: #141414;
		transform: translateY(4px);
	}
	.usr {
		display: flex;
		flex-direction: row;
		border-bottom: 1px solid black;
		align-items: center;
		padding: 0 5px;
	}
	.usr button {
		--size: 30px;
		height: var(--size);
		width: var(--size);
		background-color: rgba(255, 0, 0, 0.5);
		border: none;
		transition: all 0.1s ease-in-out;
		cursor: pointer;
		padding: 0;
		border-radius: 5px;
		font-weight: bolder;
		font-size: 1.5rem;
		line-height: var(--size);
		outline: none;
	}
	button:active {
		transform: translateY(2px) scale(0.8);
		background-color: rgba(255, 0, 0, 0.8);
	}
</style>
