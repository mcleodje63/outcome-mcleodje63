<footer>
  <div class="footerNav">
    <div class="row">
      <div class="column">
        <ul>
          <div class="footerHead">
            <li>HELPFUL PAGES</li>
          </div>
          <li><a href="/">Home</a></li>
          <li><a href="/learn">Learn</a></li>
          <li><a href="/calculator">Create a Flatter</a></li>
        </ul>
      </div>

      <div class="column">
        <ul>
          <div class="footerHead">
            <li>SUPPORT</li>
            <!-- the header is quite close to the text - add spacing -->
          </div>
          <li><a href="/contact-us"> Contact us</a></li>
          <li><a href="/help">Help</a></li>
        </ul>
      </div>
    </div>
  </div>

  <div class="socialGroup">
    <span class="dot"><a href="https://www.instagram.com/flattersnz/"><img src="instagram.png" alt="Instagram logo" class="socialIcon" /></a></span>
    <span class="dot"><a href="https://www.youtube.com/@FlatterNZ"><img src="youtube.png" alt="Youtube logo" class="socialIcon" /></a> </span>
    <span class="dot"><a href="https://pin.it/13kUE1y7V"><img src="pinterest.png" alt="Pinterest logo" class="socialIcon" /></a></span>
  </div>

  <div class="copyright">
    <a href="https://github.com/mcleodje63"> <b>&copy;Jessie Mcleod 2024</b> </a>
  </div>
</footer>

<style>
  .row {
    display: flex;
    flex-wrap: wrap;
    padding: 0 4px;
  }
  .column {
    flex: 50%;
    display: flex;
    padding: 0 4px;
  }
  .footerHead {
    color: grey;
    font-weight: 800;
  }

  .dot {
    height: 4vw;
    width: 4vw;
    background-color: #494949;
    border-radius: 100%;
    display: block;
    margin: 1vw;
    margin-right: 0;
  }

  .socialIcon {
    width: 2.5vw;
    margin: 0.75vw;
  }
  .socialGroup {
    margin-left: 35vw;
  }

  img {
    width: 3vw;
  }
  footer {
    background-color: black;
    font-family: "Inter", sans-serif;
    display: flex;
    align-items: center;
    position: relative;
    margin-bottom: 0;
    min-height: 50vh;
  }

  a {
    text-align: center;
    /* position: absolute; */
    bottom: 0;
    text-decoration: none;
    color: white;
  }
  /* .footerNav {
    display: flex;
  }
  .footerHead {
    color: #8b8b8b;
    font-weight: 900;
    margin-right: 1rem;
  } */
  ul {
    font-weight: 650;
    color: white;
    list-style: none;
    margin: 0;
    padding: 2vw;
    margin-left: 3vw;
  }
  li {
    margin: 0.8rem;
    font-size: 1.2vw;
  }
  .copyright {
    text-align: center;
    color: white;
    padding: 2vw;
    font-size: 1.2vw;
    margin-top: 19vw;
  }
</style>
