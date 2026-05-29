<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<Navigation />

<main>
  <section class="hero-panel">
    <img src="/citySkyline.JPG" alt="City skyline at sunset" />
    <div class="hero-content">
      <p class="eyebrow">Rent made easy</p>
      <h1 class="hero-title">Flatter</h1>
      <p class="hero-copy">Split rent, utilities, and flat costs with less confusion and more confidence.</p>
      <a class="primary-link" href="/calculator">Open calculator</a>
    </div>
  </section>

  <section class="section">
    <div class="page-shell">
      <h2 class="section-title">Clear numbers for shared homes.</h2>
      <p class="section-copy">Flatter keeps the weekly money conversation simple: add your rent, add flatmates, include utilities, and see what everyone should pay.</p>

      <div class="feature-grid">
        <article class="feature-card">
          <h3>Simple inputs</h3>
          <p>Enter the details your flat already knows. No spreadsheet setup or complicated formulas.</p>
        </article>
        <article class="feature-card">
          <h3>Flexible splitting</h3>
          <p>Use even shares or percentage-based rent splits when rooms or arrangements are different.</p>
        </article>
        <article class="feature-card">
          <h3>Autosaved work</h3>
          <p>Your calculator entries stay in the browser, so a refresh will not wipe your progress.</p>
        </article>
      </div>
    </div>
  </section>

  <section class="section highlight">
    <div class="page-shell split-section">
      <div>
        <p class="eyebrow">Built for everyday flatting</p>
        <h2 class="section-title">Less admin, fewer awkward chats.</h2>
        <p class="section-copy">Use Flatter when rent changes, utilities arrive, or someone moves rooms. The interface is designed for quick edits and clear totals.</p>
        <a class="secondary-link" href="/learn">Learn more</a>
      </div>
      <img src="/house.JPG" alt="House exterior" />
    </div>
  </section>
</main>

<Footer />

<style>
  .highlight {
    background: var(--surface-soft);
  }
</style>
