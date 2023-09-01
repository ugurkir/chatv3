<script lang="ts">
	import { goto } from '$app/navigation';
	import { createEventDispatcher, onMount } from 'svelte';



	let newUser: { name: string; selected: boolean }[] = [];
	let usr: string;

	onMount(() => {
		let keys = Object.keys(localStorage);
		for (let i = 0; i < keys.length; i++) {
			const key = keys[i];
			if (key.startsWith('friend: ')) {
				let name = key.replace('friend: ', '');
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

		<!-- svelte-ignore a11y-autofocus -->
		
		<button class="accSettings" 
		on:click={() => {
			let keys = Object.keys(localStorage);
		    for (let i = 0; i < keys.length; i++) {
			    const key = keys[i];
			    if (key.startsWith('currentAccount: ')) {
				    let name = key.replace('currentAccount: ', '');
                    let pas = localStorage.removeItem("currentAccount: "+name);
					goto("./Login")
				}
			}
			
		}}>Log out</button>

		

		<!-- svelte-ignore a11y-autofocus -->
		<input
			autofocus
			type="text"
			placeholder="New Chat"
			bind:value={usr}
			on:keydown={(e) => {
				if (e.key === 'Enter') {
					newUser = [...newUser, { name: usr, selected: false }];
					localStorage.setItem(`friend: ${usr}`, usr);
					usr = '';
				}
			}}
		/>
	</header>
	<nav>
		{#each newUser as user}
			<div class="usr" class:selected={user.selected}>
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
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
				<button class="delUser"
					on:click={() => {
						localStorage.removeItem(`friend: ${user.name}`);
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
		background-color: #2b2b2b;
		overflow: auto;
		text-align: center;
		user-select: none;
	}

	.closeChange{
		background-color: red;
		border: none;
		cursor: pointer;
	}

	.accChange{
		height: 100px;
		width: 25%;
		background-color: red;
		position: fixed;
		display: flex;
        justify-content: center;
        text-align:center;
        align-items: center;
        font-weight: bolder;
        font-size: 30px;
        height: 100%;
        user-select: none;
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
		border: 0px;
		background-color: black;
		color: wheat;
	}

	.accSettings{
		margin-right: 10px;
		height: 20px;
		width: fit-content;
		background-color: #272727;
		cursor: pointer;
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
	.delUser {
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
	.delUser:active {
		transform: translateY(2px) scale(0.8);
		background-color: rgba(255, 0, 0, 0.8);
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
		background: rgb(26, 25, 25); 
		border-radius: 10px;
	}

	/* Handle on hover */
	::-webkit-scrollbar-thumb:hover {
		background: rgb(37, 36, 36); 
	}
</style>
