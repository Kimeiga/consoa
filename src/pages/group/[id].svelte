<script>
  import { goto, url } from "@roxi/routify";
  export let id;

  import { Doc } from "sveltefire";

  const onImageClick = (e) => {
    e.target.style.maxWidth = "200%";
    e.target.style.maxHeight = "calc(200vh - 2rem - 2px)";
  };

  let theImage;
  let zoom = false;
</script>

<Doc path={`posts/${id}`} log let:data={post} let:ref={postRef} on:data on:ref>
  <div class="post">
    {#if post.imgURL}
      <div
        class="img-box"
        style="background-image: url({post.imgURL})"
        class:zoom
        on:click={() => (zoom = !zoom)}
      >
        <!-- <img
          src={post.imgURL}
          alt="post"
          class="post-image"
          on:click={(e) =>
            e.target == document.fullscreenElement
              ? document.exitFullscreen()
              : e.target.requestFullscreen()}
          bind:this={theImage}
        /> -->
      </div>
    {/if}

    <a href={post.imgURL}>Original Image</a>

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
  </div>

  <span slot="loading">Loading...</span>
  <span slot="fallback">Error...</span>
</Doc>

<style>
  .zoom {
    transform-origin: center top;
    /* transform: scale(2); */
    height: 200vh !important;
    cursor: zoom-out !important;
  }
  .img-box {
    max-width: 100%;
    width: auto;
    height: calc(100vh - 1rem);
    /* display: flex;
    align-items: center; */
    /* background: var(--img-url); */
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;

    transition: transform 0.25s ease;
    cursor: zoom-in;
  }
  .post-image {
    max-height: 100%;
    max-width: inherit;
    /* max-width: 100%;
    max-height: 100%; */
    /* width: 100%;
    height: auto;
    width: auto;
    max-height: calc(100vh - 1rem); */
    /* object-fit: cover; */
    /* height: 100%; */
    /* max-width: 100%;
    width: auto;
    max-height: calc(100vh - 1rem); */
    /* 
    
    max-width: 10%;
    max-height: 10%;
    transform: scale(10); */

    display: block;
    cursor: pointer;
    margin: auto;

    /* left: 50%;
    margin-left: -50vw;
    margin-right: -50vw;
    right: 50%;*/
  }
</style>
