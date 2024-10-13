<script>
    import {onMount} from "svelte"
    import { account, ID } from '../lib/appwrite';

    export let showModal;

    let user = null;
    let mode = 'login';

    onMount(async () => {
    user = await account.get();
    });

    async function login(email, password) {
        await account.createEmailPasswordSession(email, password);
        user = await account.get();
        console.log('hello');
    }

    async function register(email, password) {
        await account.create(ID.unique(), email, password);
        login(email, password);
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
        user = null;
    }


</script>

{#if showModal}
<div class="backdrop"> 
  
    <div class="modal mt-5" tabindex="-1" role="dialog" style="display: block;">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
              {#if mode=='signup'}
            <h5 class="modal-title">Create an account</h5>
            {:else}
            <h5 class="modal-title">Sign in to your account</h5>
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

{/if}

</div>

<div class="modal-footer">
	
    {#if !user}


    <button class="btn btn-dark" type="submit" data-type="login">Login</button>
    <button class="btn btn-dark" type="submit" data-type="register">Register</button>
    {/if}
    
    
    
  </div>

</form>

</div>
</div>
</div>

</div>
{/if}


