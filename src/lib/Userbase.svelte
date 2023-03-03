  <script>
	import { onMount, tick } from 'svelte';
	
	export let showModal;
	export let mode;
	export let user;
	  
	let username;
	let password;
	let error = false;
	
	let loading = false;
	  	
	function signUp(e) {
		loading = true;
		
		if(ValidateEmail(username)){
			userbase.signUp({ username, password, rememberMe: 'local' })
				.then((myuser) => {
					user = myuser;
					loading = false;
					showModal = false;
					
				})
				.catch((e) => error = e)
		}else{
			error = "invalid email";
			loading = false;
		}
		
	}
	
	function signIn(){
		loading = true;
		
		userbase.signIn({ username, password, rememberMe: 'local' })
		  .then((myuser) =>{ 
			
			  user = myuser;
			  console.log(user)
			  loading = false;
			  showModal = false;
	  		})
		  .catch((e) => {
			  error = e
			  loading = false;
		  })
	}
	
	
	
	function ValidateEmail(mail) 
	{
	  if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(mail))
	  {
		return true
	  }
	  return false
	}
	
  </script>
 
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
		<div class="modal-body">
			
			{#if mode=='signup'}
			<div class="alert alert-info text-center">
				<i class="fas fa-info-circle"></i> Create an account to save your changes<br>and export docs to PDF or HTML.
			</div>
			{/if}
		
		
  			<div id="auth-view">
			
				<form id="signup-form">
	  			<input type="text" class="form-control mb-3" required placeholder="Email" bind:value={username}>
				  
	  			<input type="password" class="form-control" required placeholder="Password" bind:value={password}>
	  		
				</form>
				{#if error}
				<div class="alert alert-danger mt-3"><b>Error:</b> {error}</div>
				{/if}
  			</div>
  			
  			</div>
		  <div class="modal-footer">
			  
			{#if mode=='signup'}
			<button type="button" class="btn btn-outline-dark me-auto" on:click={signUp}>
				{#if loading}
				<i class="fas fa-spinner fa-spin"></i> &nbsp;
				{/if}
				
				Sign Up</button>
			<div>
			Already have an account? <a class="sign-in" on:click={()=>mode='signin'}>Sign In</a>
			</div>
			{:else}
			<button type="button" class="btn btn-outline-dark me-auto" on:click={signIn}>
				{#if loading}
				<i class="fas fa-spinner fa-spin"></i> &nbsp;
				{/if}
				Sign In</button>
			<div>
			No account yet? <a class="sign-in" on:click={()=>mode='signup'}>Sign Up</a>
			</div>
			{/if}
			
			
			
		  </div>
		</div>
	  </div>
	</div>
	
 </div>

<style>
	.sign-in{
		cursor: pointer;
	}
</style>