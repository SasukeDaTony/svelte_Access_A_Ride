<script>
     let email = '';
     let password = '';
     let confirmPassword = '';
</script>



<div class="authContainer">
    <form>
        <h1>Create Log In</h1>
        <label>
            <input type="email" placeholder="E-Mail Address" />
        </label>
        <label>
            <input type="password" placeholder="Password" />
        </label> 
        <label>
            <input type="password" placeholder="Confirm Password" />
        </label>
        <button type="button">Register</button>
    </form>

</div>

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
        width:400px;
        max-width: 100%;
        margin: 0 auto;
    }

    form input {
        width:100%;
        border:none;
        background: transparent;
        color: white;
        padding: 8px 14px;
    }
    form input:focus {
        border:none;
        outline: none;
    }
form label {
    position: relative;
    border: 1px solid navy;
    border-radius: 5px;
}

    h1 {
        text-align: center;
        font-size: 3rem;
    }

    form button {
        background:blue;
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
    
</style>