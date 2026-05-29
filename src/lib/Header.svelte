<script>
  export let title = "Main heading"
  export let subtitle = "Secondary heading"
  export let eyebrow = "Flatter"
</script>

<header>
  <p class="eyebrow">{eyebrow}</p>
  <h1>{title}</h1>
  <p class="subtitle">{subtitle}</p>
</header>

<style>
  header {
    color: inherit;
  }

  h1 {
    max-width: 760px;
    margin: 10px 0 12px;
    color: inherit;
    font-size: clamp(2.7rem, 8vw, 6rem);
    font-weight: 900;
    line-height: 0.95;
    letter-spacing: 0;
  }

  .subtitle {
    max-width: 610px;
    margin: 0;
    color: currentColor;
    font-size: clamp(1.05rem, 2vw, 1.35rem);
    line-height: 1.55;
    opacity: 0.82;
  }
</style>
