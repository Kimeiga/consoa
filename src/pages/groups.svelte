<script>
  import { Collection } from "sveltefire";

  let groupText;
  let secret = false;

  $: console.log(secret);
</script>

<Collection path={"groups"} let:ref={groupsRef} let:data={groups} log>
  <span slot="loading">Loading</span>

  {#each groups as group}
    <a href="/group/{group.id}">
      {group.title}
    </a>
  {/each}
  <input type="text" bind:value={groupText} />
  <input type="checkbox" name="public" id="public" bind:checked={secret} />
  <label for="public">secret?</label>
  <button
    on:click={() =>
      groupsRef.doc(groupText).set({
        title: groupText,
        secret: secret,
      })}
  >
    Add Group
  </button>
</Collection>
