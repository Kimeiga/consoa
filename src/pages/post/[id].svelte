<script>
  import { goto } from "@roxi/routify";
  export let id;

  import { Doc, Collection } from "sveltefire";

  let zoom = false;
</script>

<Doc path={`posts/${id}`} log let:data={post} let:ref={postRef} on:data on:ref>
  <div class="post">
    {#if post.imgURL}
      {#if !zoom}
        <div
          class="img-box"
          style="background-image: url({post.imgURL})"
          on:click={() => (zoom = !zoom)}
        />
      {:else}
        <img
          class="zoom-img"
          src={post.imgURL}
          on:click={() => (zoom = !zoom)}
          alt="post"
        />
      {/if}
    {/if}

    <div class="post-creator">
      <img src={post.creatorPhoto} alt="poster's profile" />
      <p>{post.creatorName}</p>
    </div>
    <h2>{post.text}</h2>

    <small>{new Date(post.createdAt)}</small>
    <button
      on:click={() => {
        postRef.delete();
        $goto("/");
      }}>Delete</button
    >

    <Collection
      path={`posts/${id}/comments`}
      query={(ref) => ref.orderBy("createdAt", "desc")}
      let:data={comments}
      let:ref={commentsRef}
      log
    />
  </div>

  <span slot="loading">Loading...</span>
  <span slot="fallback">Error...</span>
</Doc>

<style>
  .zoom-img {
    width: 100%;
    cursor: zoom-out !important;
  }
  .img-box {
    max-width: 100%;
    width: auto;
    height: calc(100vh - 2rem);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;

    transition: transform 0.25s ease;
    cursor: zoom-in;
  }
</style>
