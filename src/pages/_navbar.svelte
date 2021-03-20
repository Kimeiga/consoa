<script>
  export let profileImgURL;
</script>

<nav>
  <a href="/"><span class="brand">Consoa</span></a>

  <a href="/user">
    <img src={profileImgURL} alt="profile" class="user-image" />
  </a>
</nav>
<hr />

<style>
  nav {
    display: flex;
    height: 2rem;
    justify-content: space-between;
    max-width: 100rem;
    margin: auto;
  }
  nav > * {
    margin: 0;
    height: 100%;
  }

  .brand {
    height: 100%;
    font-size: 1.5rem;
  }

  .user-image {
    height: 100%;
  }

  hr {
    margin: 0;
  }
</style>
