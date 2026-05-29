<script>
  let navVisible = false

  function toggleNav() {
    navVisible = !navVisible
  }
</script>

<nav class="site-nav" aria-label="Main navigation">
  <a class="brand" href="/">Flatter</a>

  <button class="nav-toggle" class:is-active={navVisible} type="button" aria-label="Toggle navigation" aria-expanded={navVisible} on:click={toggleNav}>
    <span aria-hidden="true"></span>
    <span aria-hidden="true"></span>
    <span aria-hidden="true"></span>
  </button>

  <div class="nav-links" class:is-active={navVisible}>
    <a href="/learn">Learn</a>
    <a href="/calculator">Calculator</a>
    <a href="/groceries">Groceries</a>
    <a href="/contact-us">Contact</a>
  </div>
</nav>

<style>
  .site-nav {
    position: sticky;
    top: 0;
    z-index: 20;
    display: flex;
    align-items: center;
    justify-content: space-between;
    min-height: 72px;
    padding: 0 clamp(16px, 4vw, 48px);
    border-bottom: 1px solid rgba(220, 227, 215, 0.85);
    background: rgba(247, 245, 239, 0.88);
    backdrop-filter: blur(18px);
  }

  .brand {
    color: var(--brand-dark);
    font-size: 1.45rem;
    font-weight: 900;
    text-decoration: none;
  }

  .nav-links {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .nav-links a {
    padding: 10px 14px;
    border-radius: var(--radius);
    color: var(--muted);
    font-weight: 750;
    text-decoration: none;
    transition:
      color 160ms ease,
      background-color 160ms ease;
  }

  .nav-links a:hover {
    background: var(--surface-soft);
    color: var(--brand-dark);
  }

  .nav-toggle {
    display: none;
    width: 44px;
    height: 44px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: var(--surface);
  }

  .nav-toggle span {
    display: block;
    width: 20px;
    height: 2px;
    margin: 4px auto;
    background: var(--brand-dark);
    transition: transform 180ms ease;
  }

  @media (max-width: 720px) {
    .site-nav {
      flex-wrap: wrap;
    }

    .nav-toggle {
      display: block;
    }

    .nav-links {
      display: none;
      width: 100%;
      flex-direction: column;
      align-items: stretch;
      padding: 10px 0 16px;
    }

    .nav-links.is-active {
      display: flex;
    }
  }
</style>
