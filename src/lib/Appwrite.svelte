<script>
    import {onMount} from "svelte"
    import { account, ID } from '../lib/appwrite';

    export let showModal;
    export let user;

    let mode = 'login';
    let loading = false;

    onMount(async () => {
    user = await account.get();
    });

    async function login(email, password) {
        loading = true;
        try{
        await account.createEmailPasswordSession(email, password);
        user = await account.get();
        loading = false;
        }catch(e){
            loading = false;
            alert("Invalid email or password");
        }
        
    }

    async function register(email, password) {
        loading = true;
        try{
        await account.create(ID.unique(), email, password);
            login(email, password);
            loading = false;
        }catch(e){
            loading = false;
            alert("Error creating account");
           
        }
       
    }

    function submit(e) {
        e.preventDefault();
        const formData = new FormData(e.target);
        const type = e.submitter.dataset.type;
        
        
        if (type === "login") {
            login(formData.get('email'), formData.get('password'));
        } else if (type === "register") {
            register(formData.get('email'), formData.get('password'));
        }
    }

    async function logout() {
        await account.deleteSession('current');
        user = false;
    }


    function toggleMode(){
        if(mode=='login'){
            mode='register'
        }else{
            mode='login'
        }
    }

</script>

{#if showModal}
<div class="backdrop"> 
  
    <div class="modal mt-5" tabindex="-1" role="dialog" style="display: block;">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
              {#if mode=='register'}
            <h5 class="modal-title">Register</h5>
            {:else}
            <h5 class="modal-title">Sign in</h5>
            {/if}
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" on:click={()=>showModal=false}></button>
          </div>

          <form on:submit={submit}>
          <div class="modal-body text-start">


    {#if user}
    Logged in as {user.email}
    {:else}

    <input class="form-control mb-3" type="email" placeholder="Email" name="email" required />
    <input class="form-control mb-3" type="password" placeholder="Password" name="password" required />


<div class="d-flex justify-content-end">
    {#if mode=='login'}
    <button class="btn btn-dark" type="submit" data-type="login">
        
        
        {#if loading}
        <div class="spinner-border spinner-border-sm" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          {:else}
          Sign in
          {/if}
          

       </button>
    {:else}
    <button class="btn btn-dark" type="submit" data-type="register">


        {#if loading}
        <div class="spinner-border spinner-border-sm" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          {:else}
          Register
          {/if}


    </button>
    {/if}
</div>


    {/if}

</div>

<div class="modal-footer d-flex justify-content-center">
	
    {#if !user}


    {#if mode=='login'}
    No account yet? <a on:click={toggleMode}>Register</a>

    {:else}

   Already have an account? <a on:click={toggleMode}>Sign In</a>

    {/if}

    {/if}
    
    
    
  </div>

</form>

</div>
</div>
</div>

</div>
{/if}


<style>
    .modal-footer a{
        text-decoration: underline;
        cursor: pointer;
        color: black;
    }
</style>