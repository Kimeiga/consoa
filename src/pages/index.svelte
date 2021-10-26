<script>
  import { Collection } from "sveltefire";
  export let scoped;
  $: ({ user } = scoped);
  let user;
  let postImgURL = "";
  import { User } from "sveltefire";

  import { getContext } from "svelte";
  const firebase = getContext("firebase").getFirebase();
  const googleProvider = new firebase.auth.GoogleAuthProvider();
</script>

<Collection
  path={"posts"}
  query={(ref) => ref.orderBy("createdAt", "desc")}
  let:data={posts}
  let:ref={postsRef}
  log
>
  {#if !posts.length}
    No posts yet...
  {/if}

  <User persist={localStorage} let:user let:auth>
    <nav>
      <form
        on:submit|preventDefault={() =>
          postsRef.add({
            createdAt: Date.now(),
            creatorID: user.uid,
            creatorName: user.displayName,
            creatorPhoto: user.photoURL,
            imgURL: postImgURL,
          })}
      >
        <input type="text" bind:value={postImgURL} placeholder="Add Image" />
      </form>

      <a href="/user" class="user">
        <span style="margin-right: 1ch;">User</span>
        <img src={user.photoURL} alt="profile" class="user-image" />
      </a>
    </nav>

    <div slot="signed-out">
      <nav>
        <form>
          <input
            type="text"
            bind:value={postImgURL}
            placeholder="Add Image"
            disabled
          />
        </form>

        <button
          on:click={() => {
            auth.signInWithPopup(googleProvider);
          }}
        >
          Sign In With Google
        </button>
      </nav>
    </div>
  </User>

  <section id="posts">
    {#each posts as post}
      <div class="post">
        <a href="/post/{post.id}">
          {#if post.imgURL}
            <img src={post.imgURL} alt="post" class="post-image" />
          {/if}
        </a>
      </div>
    {/each}
  </section>
  <span slot="loading">Loading posts...</span>
</Collection>

<style>
  .post-image {
    width: 100%;
    display: block;
  }

  #posts {
    column-width: calc(100px + 5vw);
    column-gap: 0;
    margin-bottom: 100px;
    column-count: 10;

    max-width: 100% !important;
    padding: 0;
  }

  nav {
    display: flex;
    height: 2rem;
    justify-content: space-between;
    max-width: 100rem;
    margin: auto;
    align-items: center;
    padding: 0 1rem;
  }
  nav > * {
    margin: 0;
  }

  .brand {
    height: 100%;
  }
  .user {
    align-items: center;
    display: flex;
  }
  .user-image {
    height: 2rem;
  }
</style>
