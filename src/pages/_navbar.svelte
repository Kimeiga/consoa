<script>
  export let profileImgURL;
</script>

<nav>
  <a href="/"><span class="brand">Consoa</span></a>

  <a href="/add"> <span>Add</span></a>

  <a href="/user">
    <span>User</span>
    <img src={profileImgURL} alt="profile" class="user-image" />
  </a>
</nav>

<style>
  nav {
    display: flex;
    height: 1rem;
    justify-content: space-between;
    max-width: 100rem;
    margin: auto;
    /* border-bottom: 1px solid white; */
  }
  nav > * {
    margin: 0;
    height: 100%;
  }

  .brand {
    height: 100%;
    /* font-size: 1rem; */
  }

  .user-image {
    height: 100%;
  }
</style>
