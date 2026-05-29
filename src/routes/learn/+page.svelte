<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<Navigation />

<main>
  <section class="hero-panel compact">
    <img src="/chchStreet.JPG" alt="Christchurch street" />
    <div class="hero-content">
      <p class="eyebrow">How it helps</p>
      <h1 class="hero-title">All sorted.</h1>
      <p class="hero-copy">Bring rent, utilities, and flatmate shares into one clear weekly view.</p>
    </div>
  </section>

  <section class="section">
    <div class="page-shell">
      <h2 class="section-title">Designed around the real flat workflow.</h2>
      <p class="section-copy">The calculator works best when you want a quick answer, but the structure also supports the small changes that happen in shared homes.</p>

      <div class="feature-grid">
        <article class="feature-card image-card">
          <img src="/skytower.JPG" alt="Auckland skyline at night" />
          <h3>Know what you owe</h3>
          <p>Turn weekly costs into clear individual amounts.</p>
        </article>
        <article class="feature-card image-card">
          <img src="/concertMtSmart.jpg" alt="Concert crowd at Mt Smart" />
          <h3>Save time</h3>
          <p>Spend less time chasing numbers and more time doing what you enjoy.</p>
        </article>
        <article class="feature-card image-card">
          <img src="/centralCity.JPG" alt="City shops" />
          <h3>Update anytime</h3>
          <p>Edit names, rent, percentages, and utilities as your flat changes.</p>
        </article>
      </div>
    </div>
  </section>

  <section class="section soft">
    <div class="page-shell split-section reverse">
      <img src="/mountain.png" alt="Mountain landscape" />
      <div>
        <p class="eyebrow">For students and shared homes</p>
        <h2 class="section-title">Clear enough for everyone.</h2>
        <p class="section-copy">Flatter keeps the language plain and the calculations visible so each person can understand where their amount came from.</p>
      </div>
    </div>
  </section>
</main>

<Footer />

<style>
  .compact {
    min-height: clamp(360px, 54vh, 560px);
  }

  .image-card {
    overflow: hidden;
    padding: 0;
  }

  .image-card img {
    width: 100%;
    height: 210px;
    object-fit: cover;
  }

  .image-card h3,
  .image-card p {
    padding-inline: 22px;
  }

  .image-card h3 {
    margin-top: 20px;
  }

  .image-card p {
    padding-bottom: 24px;
  }

  .soft {
    background: var(--surface-soft);
  }
</style>
