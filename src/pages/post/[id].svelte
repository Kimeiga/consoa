<script>
  import { goto } from "@roxi/routify";
  export let id;

  import { Doc, Collection } from "sveltefire";
</script>

<Doc path={`posts/${id}`} log let:data={post} let:ref={postRef} on:data on:ref>
  <div class="post">
    {#if post.imgURL}
      <div class="img-box" style="background-image: url({post.imgURL})" />
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
  .img-box {
    max-width: 100%;
    width: auto;
    height: 100vh;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;
  }
</style>
