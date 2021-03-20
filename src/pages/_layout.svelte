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

  <main>
    <slot scoped={{ user, auth }} />
  </main>

  <div slot="signed-out">
    <h1>Consoa</h1>
    <h3>haki's social network ;)</h3>
    <hr />
    <button
      on:click={() => {
        auth.signInWithPopup(googleProvider);
        postsRef.add({
          text: postText,
          createdAt: Date.now(),
          creatorID: user.uid,
        });
      }}
    >
      Sign In With Google
    </button>
  </div>
</User>

<style>
  main {
    max-width: 100rem;
    margin: auto;
  }
</style>
