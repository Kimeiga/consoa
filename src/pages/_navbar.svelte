<script>
  export let profileImgURL;
</script>

<nav>
  <a href="/"><span class="brand">Consoa</span></a>

  <a href="/add"><span>Add</span></a>
  
  <a href="/groups"><span>Groups</span></a>

  <a href="/user" class="user">
    <span>User</span>
    <img src={profileImgURL} alt="profile" class="user-image" />
  </a>
</nav>

<style>
  nav {
    display: flex;
    height: 2rem;
    justify-content: space-between;
    max-width: 100rem;
    margin: auto;
    align-items: center;
    padding: 0 1rem;
  }
  nav > * {
    margin: 0;
  }

  .brand {
    height: 100%;
  }
  .user {
    align-items: center;
    display: flex;
  }
  .user-image {
    height: 2rem;
  }
</style>
