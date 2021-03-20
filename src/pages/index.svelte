<script>
  import { metatags } from "@roxi/routify";
  metatags.title = "My Routify app";
  metatags.description = "Description coming soon...";

  import { User, Collection } from "sveltefire";

  let postText = "";
  let postImgURL = "";

  export let scoped;
  $: ({ user, auth } = scoped);
  let user;
  let auth;
  $: console.log(user);

  const onImageLoad = (e) => {
    console.log(e);
  };
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

  <section>
    <!-- class:wide="{current === 'foo'}"  -->
    {#each posts as post}
      <div class="post">
        <!-- <p>
          {JSON.stringify(post, Objct.keys(post).sort())}
        </p> -->
        <!-- <div class="post-creator">
          <img src={post.creatorPhoto} alt="poster's profile" />
          <p>{post.creatorName}</p>
        </div> -->
        <!-- <h2>{post.text}</h2> -->
        <a href="/post/{post.id}">
          {#if post.imgURL}
            <img
              src={post.imgURL}
              alt="post"
              class="post-image"
              on:load={(e) => onImageLoad(e)}
            />
          {/if}
        </a>
        <!-- <small>{new Date(post.createdAt)}</small>
        <button on:click={() => post.ref.delete()}>Delete</button> -->
      </div>
    {/each}
  </section>

  <input type="text" bind:value={postText} />
  <input type="text" bind:value={postImgURL} />

  <button
    on:click={() =>
      postsRef.add({
        text: postText,
        createdAt: Date.now(),
        creatorID: user.uid,
        creatorName: user.displayName,
        creatorPhoto: user.photoURL,
        imgURL: postImgURL,
      })}
  >
    Add Post
  </button>

  <span slot="loading">Loading posts...</span>
</Collection>

<style>
  /* section {
    display: grid;
    grid-template-columns: repeat(8, minmax(0, 1fr));
  }

  .post-creator {
    display: flex;
    height: 2rem;
  }
  .post-creator > p {
    margin: 0;
  }
  .post {
    aspect-ratio: 1 / 1;
    height: 12.5vw;
  }

  @media only screen and (max-width: 900px) {
    .post {
      height: 20vw;
    }
    section {
      grid-template-columns: repeat(5, minmax(0, 1fr));
    }
  }

   */
  .post-image {
    object-fit: cover;
    height: 100%;
    width: 100%;
  }

  section {
    display: grid;
    /* calc(120px + 5vw) */
    /* minmax(1fr, 3fr) */
    grid-template-columns: repeat(auto-fit, minmax(calc(120px + 5vw), 1fr));
    grid-auto-rows: 1fr;
  }

  section::before {
    content: "";
    width: 0;
    padding-bottom: 100%;
    grid-row: 1 / 1;
    grid-column: 1 / 1;
  }

  section > *:first-child {
    grid-row: 1 / 1;
    grid-column: 1 / 1;
  }

  /* Just to make the grid visible */
</style>
