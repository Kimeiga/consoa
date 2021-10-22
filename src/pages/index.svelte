<script>
  import { metatags } from "@roxi/routify";
  metatags.title = "consoa";
  metatags.description = "Description coming soon...";

  import { Collection } from "sveltefire";

  export let scoped;
  $: ({ user } = scoped);
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
</style>
