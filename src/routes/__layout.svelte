<script>
  import {auth} from '../firebase';
  import { onMount } from 'svelte';
  import authStore from '../stores/authStore';
  import {onAuthStateChanged } from "firebase/auth";
 
  onMount(() => {
    onAuthStateChanged(auth, async (user) => {
      authStore.set({
        isLoggedIn: user !== null,
        user,
        firebaseControlled: true,
      }); 
    }); 
  });
</script>
<slot />
