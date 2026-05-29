<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<Navigation />

<main>
  <section class="hero-panel compact">
    <img src="/yellowHouse.JPG" alt="Yellow house illustration" />
    <div class="hero-content">
      <p class="eyebrow">Credits</p>
      <h1 class="hero-title">The legal stuff.</h1>
      <p class="hero-copy">Attribution and source links for the project assets.</p>
    </div>
  </section>

  <section class="section">
    <div class="page-shell legal-list">
      <a href="https://github.com/craighead-diocesan-school/sveltekit-template.git">SvelteKit template by Mr Hullen</a>
      <a href="https://www.flaticon.com/authors/freepik">Freepik icons from Flaticon</a>
      <a href="https://www.flaticon.com/free-icons/pinterest">Pinterest icons by Pixel perfect on Flaticon</a>
      <a href="https://www.canva.com/templates/EAGHQZdbWW4-yellow-and-brown-minimalist-vintage-good-morning-animated-instagram-post/">Canva template credits</a>
      <a href="https://nz.pinterest.com/jessieeemcleod/?invite_code=e72319c28e8c4f84baba0bae3aca7bac&sender=849913898336554695">Photos by Jessie McLeod</a>
    </div>
  </section>
</main>

<Footer />

<style>
  .compact {
    min-height: clamp(360px, 54vh, 560px);
  }

  .legal-list {
    display: grid;
    gap: 12px;
    max-width: 760px;
  }

  .legal-list a {
    padding: 18px 20px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: var(--surface);
    color: var(--brand-dark);
    font-weight: 750;
    text-decoration: none;
    transition:
      border-color 160ms ease,
      transform 160ms ease,
      box-shadow 160ms ease;
  }

  .legal-list a:hover {
    border-color: rgba(49, 95, 60, 0.35);
    box-shadow: 0 14px 34px rgba(35, 53, 38, 0.08);
    transform: translateY(-2px);
  }
</style>
