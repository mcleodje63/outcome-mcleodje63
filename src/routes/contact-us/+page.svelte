<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<Navigation />

<main>
  <section class="section contact-section">
    <div class="page-shell split-section">
      <div>
        <p class="eyebrow">Support</p>
        <h1 class="section-title">Need to reach us?</h1>
        <p class="section-copy">Send a message or connect online. We keep support simple and direct.</p>
      </div>

      <div class="contact-card">
        <a href="mailto:flatteremail@gmail.com">
          <span>Email</span>
          flatteremail@gmail.com
        </a>
        <a href="https://www.linkedin.com/in/flatter-admin-201904324/">
          <span>LinkedIn</span>
          Flatter admin
        </a>
      </div>
    </div>
  </section>
</main>

<Footer />

<style>
  .contact-section {
    min-height: 62vh;
    display: flex;
    align-items: center;
  }

  .contact-card {
    display: grid;
    gap: 14px;
    padding: 24px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: var(--surface);
    box-shadow: var(--shadow);
  }

  .contact-card a {
    display: grid;
    gap: 5px;
    padding: 18px;
    border-radius: var(--radius);
    background: var(--surface-soft);
    color: var(--brand-dark);
    font-weight: 850;
    text-decoration: none;
    transition:
      background-color 160ms ease,
      transform 160ms ease;
  }

  .contact-card a:hover {
    background: #e0eadc;
    transform: translateY(-2px);
  }

  .contact-card span {
    color: var(--muted);
    font-size: 0.82rem;
    font-weight: 750;
    text-transform: uppercase;
  }
</style>
