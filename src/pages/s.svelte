<!-- secret page -->
<script>
  import { metatags } from "@roxi/routify";
  metatags.title = "consoa";
  metatags.description = "Description coming soon...";

  import { Collection } from "sveltefire";

  let postText = "";
  let postImgURL = "";

  export let scoped;
  $: ({ user } = scoped);
  let user;

  const onImageLoad = (e) => {
    console.log(e);
  };
</script>

<Collection
  path={"s"}
  query={(ref) => ref.orderBy("createdAt", "desc")}
  let:data={posts}
  let:ref={postsRef}
  log
>
  {#if !posts.length}
    No posts yet...
  {/if}

  <section id="posts">
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
        <a href="/s/{post.id}">
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
  <span slot="loading">Loading posts...</span>
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
</style>
