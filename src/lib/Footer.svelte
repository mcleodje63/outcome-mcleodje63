<footer>
  <div class="footer-inner">
    <div>
      <a class="footer-brand" href="/">Flatter</a>
      <p>Simple flat cost splitting for shared homes.</p>
    </div>

    <div class="footer-links">
      <div>
        <h2>Pages</h2>
        <a href="/">Home</a>
        <a href="/learn">Learn</a>
        <a href="/calculator">Calculator</a>
      </div>

      <div>
        <h2>Support</h2>
        <a href="/contact-us">Contact us</a>
        <a href="/legalities">Legalities</a>
      </div>
    </div>

    <div class="social-links" aria-label="Social links">
      <a href="https://www.instagram.com/flattersnz/" aria-label="Instagram"><img src="/instagram.png" alt="" /></a>
      <a href="https://www.youtube.com/@FlatterNZ" aria-label="YouTube"><img src="/youtube.png" alt="" /></a>
      <a href="https://pin.it/13kUE1y7V" aria-label="Pinterest"><img src="/pinterest.png" alt="" /></a>
    </div>
  </div>

  <div class="copyright">
    <a href="https://github.com/mcleodje63">&copy; Jessie Mcleod 2024</a>
  </div>
</footer>

<style>
  footer {
    border-top: 1px solid rgba(255, 255, 255, 0.08);
    background: #111a13;
    color: white;
  }

  .footer-inner {
    display: grid;
    grid-template-columns: minmax(220px, 1.1fr) minmax(260px, 1fr) auto;
    gap: 36px;
    width: min(1120px, calc(100% - 32px));
    margin: 0 auto;
    padding: 56px 0 34px;
  }

  .footer-brand {
    display: inline-block;
    margin-bottom: 10px;
    color: white;
    font-size: 1.6rem;
    font-weight: 900;
    text-decoration: none;
  }

  p {
    max-width: 320px;
    margin: 0;
    color: rgba(255, 255, 255, 0.72);
    line-height: 1.6;
  }

  .footer-links {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 28px;
  }

  h2 {
    margin: 0 0 12px;
    color: var(--accent);
    font-size: 0.78rem;
    font-weight: 850;
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  .footer-links a {
    display: block;
    margin: 8px 0;
    color: rgba(255, 255, 255, 0.78);
    text-decoration: none;
    transition: color 160ms ease;
  }

  .footer-links a:hover {
    color: white;
  }

  .social-links {
    display: flex;
    gap: 10px;
  }

  .social-links a {
    display: grid;
    width: 44px;
    height: 44px;
    place-items: center;
    border-radius: var(--radius);
    background: rgba(255, 255, 255, 0.08);
    transition:
      background-color 160ms ease,
      transform 160ms ease;
  }

  .social-links a:hover {
    background: rgba(255, 255, 255, 0.15);
    transform: translateY(-2px);
  }

  .social-links img {
    width: 24px;
    height: 24px;
    object-fit: contain;
  }

  .copyright {
    width: min(1120px, calc(100% - 32px));
    margin: 0 auto;
    padding: 18px 0 28px;
    border-top: 1px solid rgba(255, 255, 255, 0.08);
  }

  .copyright a {
    color: rgba(255, 255, 255, 0.62);
    text-decoration: none;
  }

  @media (max-width: 820px) {
    .footer-inner {
      grid-template-columns: 1fr;
    }
  }
</style>
