<!-- Javascript -->

<script>
  import { authHandlers } from "../store/store";

  let email = "";
  let password = "";
  let confirmPassword = "";
  let error = false;
  let register = false;
  let authentication = false;

  async function handleAuthenticate() {
    if (authentication) {
      return;
    }
    if (!email || !password || (register && !confirmPassword)) {
      error = true;
      return;
    }
    authentication = true;
    try {
      if (!register) {
        await authHandlers.login(email, password);
      } else {
        await authHandlers.signup(email, password);
      }
    } catch (err) {
      console.log("Authentication error HTTP 401 Failed Authorization", err);
      error = true;
    }
  }

  function handleRegister() {
    register = !register;
    error = false;
  }
</script>

<!-- HTML -->

<div class="authContainer">
  <form>
    <h1>{register ? "Create Account" : "Log In"}</h1>
    {#if error}
      <div class="bug">
        <p class="error">OOPS! Something Went Wrong...</p>
        <img
          src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQB_2cn0ZZx7JOKko95Q0c4ctpHFwXmhp769Q&usqp=CAU"
          alt="bug"
        />
      </div>
    {/if}
    <label>
      <p class={email ? "above" : "center"}>Email</p>
      <input type="email" placeholder="E-Mail Address" bind:value={email} />
    </label>

    <label>
      <p class={password ? "above" : "center"}>Password</p>
      <input type="password" placeholder="Password" bind:value={password} />
    </label>
    {#if register}
      <label>
        <p class={confirmPassword ? "above" : "center"}>Confirm Password</p>
        <input
          type="password"
          placeholder="Confirm Password"
          bind:value={confirmPassword}
        />
      </label>
    {/if}
    <button on:click={handleAuthenticate} type="button">
      {#if authentication}
        <i class="fa-solid fa-spinner"></i>
      {:else}
        {register ? "Register" : "Log In"}
      {/if}
    </button>
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
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    flex: 1;
    padding: 24px;
  }

  .bug {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
  }

  .bug img {
    width: 75px;
    height: 75px;
    border-radius: 50px;
  }

  form {
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  form,
  .options {
    width: 400px;
    max-width: 100%;
    margin: 0 auto;
  }

  h1 {
    text-align: center;
    font-size: 3rem;
  }

  form input {
    width: 100%;
    border: none;
    background: transparent;
    color: white;
    padding: 14px;
  }

  form input:focus {
    border: none;
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
    background: navy;
    color: white;
    border: none;
    padding: 14px 0;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1rem;
    display: grid;
    place-items: center;
  }

  .fa-spinner {
    animation: spin 2s infinite;
  }

  @keyframes fa-spinner {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }

  form button:hover {
    background: rgb(125, 125, 197);
  }

  .above,
  .center {
    position: absolute;
    transform: translateY(-50%);
    pointer-events: none;
    color: white;
    border-radius: 4px;
    padding: 0 6px;
    font-size: 0.8rem;
  }

  .above {
    top: 0;
    left: 24px;
    background: rgb(125, 125, 197);
    border: 1px solid navy;
    font-size: 0.7rem;
  }

  .center {
    top: 50%;
    left: 6px;
    border: 1px solid transparent;
    opacity: 0;
  }
  .error {
    color: coral;
    font-size: 0.9rem;
    text-align: center;
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
    width: fit-content;
    margin: 0 auto;
    padding: 0 8px;
  }

  .options > p::after,
  .options > p::before {
    position: absolute;
    top: 50%;
    content: "";
    transform: translateY(-50%);
    width: 100vw;
    height: 1.5px;
    background: white;
  }
  .options > p::after {
    right: 100%;
  }
  .options > p::before {
    left: 100%;
  }

  .options div {
    display: flex;
    align-items: center;
    gap: 8px;
    justify-content: center;
  }

  .options div p:last-of-type {
    color: cyan;
    cursor: pointer;
  }
</style>
