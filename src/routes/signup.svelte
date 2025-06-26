<script>
  import {createUserWithEmailAndPassword} from 'firebase/auth';
  import {auth} from '../firebase';
  import { goto } from '$app/navigation';
  import authStore from '../stores/authStore';
  import { onDestroy } from 'svelte';
  
  let email;
  let password;
  
  async function register(){
     try{ 
       await createUserWithEmailAndPassword(auth, email, password);
     }catch(error){
       console.log(error);
   }
  }
  
   const sub = authStore.subscribe(async (info) => {
      if (info.isLoggedIn) {
        await goto('/login');
      }
   });
  
  onDestroy(() => {
    sub();
  });
</script>

<div class="card mt-5 m-auto text-center" style="width: 24em;">
    <div class="card-body">
   	 <h5 class="card-title">Welcome</h5>
   	 <p class="card-text text-muted">Please sign up to continue...</p>
   	 <form class="form-floating" on:submit|preventDefault={register}>
   		 <div class="mb-3">
   			<label for="emailInput" class="form-label">Email address</label>
   			<input bind:value={email} type="email" class="form-control" id="emailInput" />
   		 </div>
   		 <div class="mb-3">
   			 <label for="passwordInput" class="form-label">Password</label>
   			 <input
   				 bind:value={password}
   				 type="password"
   				 class="form-control"
   				 id="passwordInput"
   				 aria-describedby="passwordHelp"
   			 />
   		 </div>
   		 <div id="passwordHelp" class="form-text">
   			 Your password must be at least 6 characters long.
   		 </div>
   		 <button type="submit" class="btn btn-dark mt-3">Sign Up</button>
   	 </form>
    </div>
</div>

<div class="text-center mt-3">
    <a href="/login" class="text-muted">Already have an account? Click here to login.</a>
</div>
