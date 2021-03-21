<script>
  import { Collection } from "sveltefire";

  let postText = "";
  let postImgURL = "";

  export let scoped;
  $: ({ user } = scoped);
  let user;
</script>

<Collection path={"posts"} let:ref={postsRef} log>
  <span slot="loading">Loading</span>

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
