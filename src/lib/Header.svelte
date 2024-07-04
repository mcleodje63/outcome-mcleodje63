<script>
  export let title
  export let subtitle
</script>

<header class="hero">
  <div class="hero-body">
    <h3 class="logo">Flatter</h3>
    <h1>{title}</h1>
    <p>{subtitle}</p>
  </div>
</header>

<style>
  .logo {
    color: white;
    font-weight: 600;
    font-family: "Inter", sans-serif;
  }
  h1 {
    color: white;
    font-weight: 900;
    font-family: "Inter", sans-serif;
    font-size: 3em;
  }
  p {
    color: white;
    font-weight: 350;
    font-family: "Inter", sans-serif;
    font-size: 2em;
  }
</style>
