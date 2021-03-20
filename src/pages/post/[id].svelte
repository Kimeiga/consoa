<script>
  import { goto, url } from "@roxi/routify";
  export let id;

  import { Doc } from "sveltefire";

  const onImageClick = (e) => {
    e.target.style.maxWidth = "200%";
    e.target.style.maxHeight = "calc(200vh - 2rem - 2px)";
  };

  let theImage;
</script>

<Doc path={`posts/${id}`} log let:data={post} let:ref={postRef} on:data on:ref>
  <div class="post">
    <!-- <p>
				{JSON.stringify(post, Objct.keys(post).sort())}
			</p> -->

    {#if post.imgURL}
      <img
        src={post.imgURL}
        alt="post"
        class="post-image"
        on:click={(e) =>
          e.target == document.fullscreenElement
            ? document.exitFullscreen()
            : e.target.requestFullscreen()}
        bind:this={theImage}
      />
    {/if}

    <div class="post-creator">
      <img src={post.creatorPhoto} alt="poster's profile" />
      <p>{post.creatorName}</p>
    </div>
    <h2>{post.text}</h2>

    <small>{new Date(post.createdAt)}</small>
    <button on:click={() => postRef.delete()}>Delete</button>
  </div>

  <span slot="loading">Loading...</span>
  <span slot="fallback">Error...</span>
</Doc>

<style>
  /* section {
    display: grid;
    grid-template-columns: repeat(6, minmax(0, 1fr));
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
  } */

  .post-image {
    /* object-fit: cover; */
    /* height: 100%; */
    max-width: 100%;
    width: auto;
    max-height: calc(100vh - 2rem - 2px);
    display: block;
    cursor: pointer;
    margin: auto;

    /* left: 50%;
    margin-left: -50vw;
    margin-right: -50vw;
    right: 50%;*/
  }
</style>
