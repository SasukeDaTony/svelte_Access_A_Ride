<!-- Javascript -->

<script>
     let email = '';
     let password = '';
     let confirmPassword = '';
     let error = false;
     let register = false;

     function handleAuthenticate() {
        if (!email || !password || (register && !confirmPassword)) {
            error = true;
            return;
        }
     }

     function handleRegister() {
        register = !register;

     }
</script>



<!-- HTML -->

<div class="authContainer">
    <form>
        <h1>{register ? "Create Account" : "Log In"}</h1>
        {#if error}
       <p class="error">The information you have entered is incorrect</p>
       {/if}
        <label>
            <p class={email ? "above" : "center"}>Email</p>
            <input type="email" placeholder="E-Mail Address" bind:value={email} />
        </label>
        
        <label>
            <p class={password ? "above" : "center"}>Password</p>
            <input type="password" placeholder="Password" bind:value={password}/>
        </label> 
       {#if register}
        <label>
            <p class={confirmPassword ? "above" : "center"}>Confirm Password</p>
            <input type="password" placeholder="Confirm Password" bind:value={confirmPassword}/>
        </label>
        <!-- <button>I'm A New Patient</button>
        <button>I'm A New Driver</button> -->
        {/if}
        <button type="button">{register ? "Register" : 'Log In'}</button>
    </form>
    <div class="options">
        <p>Or</p>
        {#if register}
        <div>
            <p>Already have an account?</p>
            <p on:click={handleRegister} on:keydown={() => {}}>Log In</p>
        </div>
        {:else}
        <div>
            <p>Don't have an account?</p>
            <p on:click={handleRegister} on:keydown={() => {}}>Register</p>
        </div>
        {/if}
    </div>
</div>








<!-- CSS -->

<style>
    .authContainer {
        display:flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        flex: 1;
        padding: 24px;
    }

    form {
        display: flex;
        flex-direction: column;
        gap:14px;
    }

    form,
    .options {
        width:400px;
        max-width: 100%;
        margin: 0 auto;
    }

    h1 {
        text-align: center;
        font-size: 3rem;
    }

    form input {
        width:100%;
        border:none;
        background: transparent;
        color: white;
        padding:14px;
    }

    form input:focus {
        border:none;
        outline: none;
    }

    form label:focus-within {
        border-color: blue;
    }

    form label {
    position: relative;
    border: 1px solid navy;
    border-radius: 5px;
    }

  
    form button {
        background:navy;
        color:white;
        border: none;
        padding: 14px 0;
        border-radius: 5px;
        cursor: pointer;
        font-size: 1rem;
    }

    form button:hover {
        background: rgb(125, 125, 197);
    }


    .above,
    .center {
        position:absolute;
        transform:translateY(-50%);
        pointer-events: none;
        color:white;
        border-radius:4px;
        padding:0 6px;
        font-size: 0.8rem;
    }

    .above {
        top:0;
        left: 24px;
        background: rgb(125, 125, 197);
        border: 1px solid navy;
        font-size: 0.7rem;
    }
    
    .center {
        top:50%;
        left:6px;
        border: 1px solid transparent;
        opacity:0;
    }
    .error {
        color: coral;
        font-size: 0.9rem;
    }

    .options {
        padding: 14px 0;
        overflow: hidden;
        font-size: 0.9rem;
        display: flex;
        flex-direction: column;
        gap: 4px;
    }
    
    .options > p {
        position: relative;
        text-align: center;
        width:fit-content;
        margin: 0 auto;
        padding: 0 8px;
      
    }

    .options > p::after,
    .options > p::before {
        position: absolute;
        top: 50%;
        content: '';
        transform: translateY(-50%);
        width: 100vw;
        height: 1.5px;
        background: white;
    }
    .options > p::after {
        right:100%;
    }
    .options > p::before {
        left:100%;
    }


    .options div {
        display: flex;
        align-items: center;
        gap:8px;
        justify-content: center;
    }

    .options div p:last-of-type {
    color:cyan;
    cursor: pointer;
    }
</style>