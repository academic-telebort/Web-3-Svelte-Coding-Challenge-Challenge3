<script>
 import {auth} from '../firebase';
 import {onDestroy} from 'svelte';
 import { signOut} from 'firebase/auth';
 import authStore from '../stores/authStore';
 import { goto } from '$app/navigation';
  
 async function logout(){
      try{ 
        await  signOut(auth);
        $authStore.isLoggedIn = false;
        $authStore.firebaseControlled = false;
     }catch(err){
       console.log(err);
     }
  }
  
 const sub = authStore.subscribe(async ({isLoggedIn,firebaseControlled}) => {
    if (!isLoggedIn && firebaseControlled) {
      await goto('/login');
    }
  });

  onDestroy(() => {
      sub();
  });
</script>
