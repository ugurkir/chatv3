<script lang="ts">
	import { goto } from "$app/navigation";

    let logName = "";
    let logPass = "";

</script>

<div class="loginPage">

    <div class="logIn">

        <p>Log in!</p>

        <input placeholder="User name:" type="text" bind:value={logName}/>

        <input placeholder="Password:" type="password" bind:value={logPass}/>
        <button on:click={() => {
            let keys = Object.keys(localStorage);
		    for (let i = 0; i < keys.length; i++) {
			    const key = keys[i];
			    if (key.startsWith('account: ')) {
				    let name = key.replace('account: ', '');
                    let pas = localStorage.getItem("account: "+name);

                    console.log(pas);
				    if (name !== logName) {
					    alert("aa");
                        logName = "";
                        logPass = "";
				    }
                    else if( pas !== logPass ) {
                        alert("olmaaaz");
                        logPass = "" ;
                        logName = "";
                    }
                    else {
                        localStorage.setItem(`currentAccount: ${logName}`, logPass);
                        goto("/");
                    }
			    }
		    }
        }}>Log in</button>

       

    </div>

</div>

<style>

   


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

    .loginPage{

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
    .logIn input{
        padding: 10px;
        transition: border-color 0.5s;
        width: 200px;
        justify-content: center;
        align-items: center;
        user-select: none;
        margin: 20px;
    }

    .logIn{

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