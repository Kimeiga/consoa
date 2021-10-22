<script>
  import { metatags } from "@roxi/routify";
  metatags.title = "My Routify app";
  metatags.description = "Description coming soon...";

  import { goto } from "@roxi/routify";
  export let scoped;
  $: ({ user, auth } = scoped);
  let user;
  let auth;
</script>

<div style="text-align: center;">
  <a href="/"> <h5>Return Home</h5></a>

  <h1>Hi {user.displayName}</h1>

  <img src={user.photoURL} alt="profile" />

  <h2>Name</h2>
  <p>{user.displayName}</p>
  <h2>Email</h2>
  <p>{user.email}</p>

  <button
    on:click={() => {
      auth.signOut();
      $goto("/");
    }}>Sign Out</button
  >
</div>
