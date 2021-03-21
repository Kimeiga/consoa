<script>
  import { metatags } from "@roxi/routify";
  metatags.title = "consoa";
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
  .post-image {
    object-fit: cover;
    height: 100%;
    width: 100%;
  }

  section {
    display: grid;
    /* calc(120px + 5vw) */
    /* minmax(1fr, 3fr) */
    grid-template-columns: repeat(auto-fit, minmax(calc(100px + 5vw), 1fr));
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
</style>
