<script lang="ts">
	import { goto } from "$app/navigation";





    let userName = "";
    let userPass = "";

    let nameValid = true;
    let passValid = true;

   
</script>

<div class="signUpPage">

    <div class="signUp">

        <p>Create an account!</p>
        <!-- svelte-ignore a11y-autofocus -->
        <input 
        placeholder="User name:"  
        type="text"
        class:custom-input={true}
        class:invalidName={!nameValid}
        autofocus 
        bind:value={userName}
        >

        <input 
        placeholder="Password:" 
        type="password" 
        class:custom-input={true}
        class:invalidPass={!passValid} 
        bind:value={userPass}
        >

        <button on:click={() => {
            
            if (userName == "" || Number()) {

                nameValid = false;
                passValid = true;

            } 

            else if (userPass == "") {

                passValid = false;
                nameValid = true;

            } 

            else{

			localStorage.setItem(`account: ${userName}`, userPass);
            passValid = true;
            nameValid = true;
            userName = "";
            userPass = "";
            goto("/Login");

            }
            
        }}>Sign Up!</button>

    </div>

</div>

<style>

    .custom-input {

        padding: 10px;
        transition: border-color 0.5s;
        width: 200px;
        justify-content: center;
        align-items: center;
        user-select: none;
        margin: 20px;

    }

    .custom-input.invalidName {

        border-color: red;

    }

    .custom-input.invalidPass {

        border-color: red;

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
	
	@media (max-width: 600px) {

		div {
			display: flex;
			flex-direction: column;
		}

	}

    .signUpPage{

        display: flex;
        flex-direction: column;
        justify-content: center;
        text-align:center;
        align-items: center;
        font-weight: bolder;
        font-size: 30px;
        height: 100%;
        user-select: none;

    }

    .signUp{

        background-color: rgb(56, 56, 56);
        height: 60%;
        width: 50%;
        display: flex;
        justify-content: center; 
        align-items: center; 
        flex-direction: column;
        position: relative;
    
    }

</style>