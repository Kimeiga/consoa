<!-- routify:options preload="proximity" -->
<script>
  import { User } from "sveltefire";
  import Navbar from "./_navbar.svelte";

  import { getContext } from "svelte";
  const firebase = getContext("firebase").getFirebase();
  const googleProvider = new firebase.auth.GoogleAuthProvider();
</script>

<User persist={localStorage} let:user let:auth>
  <Navbar profileImgURL={user.photoURL} />

  <slot scoped={{ user, auth }} />

  <div slot="signed-out">
    <h1>Consoa</h1>
    <h3>haki's social network ;)</h3>
    <hr />
    <button
      on:click={() => {
        auth.signInWithPopup(googleProvider);
      }}
    >
      Sign In With Google
    </button>
  </div>
</User>

<style>
</style>
