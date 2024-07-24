<script>
  export let title = "Main heading"
  export let subtitle = "Secondary heading"
</script>

<header class="hero">
  <link rel="icon" type="image/x-icon" href="flatterLogo" />
  <div class="hero-body">
    <h3 class="logo">Flatter</h3>
    <h1>{title}</h1>
    <p>{subtitle}</p>
  </div>
</header>

<style>
  .logo {
    color: white;
    font-weight: 650;
    font-family: "Inter", sans-serif;
    font-size: 1.5vw;
  }
  h1 {
    color: white;
    font-weight: 900;
    font-family: "Inter", sans-serif;
    font-size: 3.7vw;
  }
  p {
    color: white;
    font-weight: 400;
    font-family: "Inter", sans-serif;
    font-size: 2vw;
  }
</style>
