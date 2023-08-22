<script lang="ts">
	import { onMount } from "svelte";
    

    let newUser: any[] = [];
    let usr:string;

    onMount(() => {
        let keys = Object.keys(localStorage);
        for (let i = 0; i < keys.length; i++) {
            const key = keys[i];
            if (key.startsWith("user: ")) {
                let name = key.replace("user: ", "");
                if (name !== ""){
                    newUser = [...newUser, name];
                }
            }
            
        }
    })
        
    


</script>
<div class="allusr">
    <header>
        <input type="text" placeholder="New User" bind:value={usr} on:keydown={(e) => {
            if(e.key === 'Enter'){
                newUser = [...newUser, usr];
                localStorage.setItem("user: " + usr, usr);
                usr = "";
            }
        }}/>
    </header>   
    <nav>  
        {#each newUser as users}
            <p class="users">{users}</p>
        {/each}
    </nav>

</div>

<style>

    .allusr{

        display: grid;
        grid-template-columns: 1;
        gap: 10px;
        grid-auto-rows: 50px;
        background-color: rgb(32, 32, 32);
        overflow: auto;
    
    }

    header{

        grid-area: 1 / 1 / 1 / 1;
        grid-row: 1;
        height: 70px;
        border-bottom: 1px solid black;
    

    }
    header input{

        margin-top: 35px;
        margin-left: 40px;

    }

    .users{

        grid-row: 1 / 7;
        grid-column: 1;
        color: rgb(209, 203, 203);
        height: 40px;
        border-bottom: 1px solid black;
        user-select: none;
       
    }

    .users:hover{

        background-color: rgb(34, 34, 34);
	    color: rgb(75, 75, 75);
	    cursor: pointer;

    }

    .users:active{

        background-color: #141414;
	    transform: translateY(4px);

    }

</style>

