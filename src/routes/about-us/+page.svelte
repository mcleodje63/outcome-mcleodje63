<script>
  import Navigation from "$lib/Navigation.svelte"
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
</script>

<Navigation />
<div class="header"><Header title="About us" subtitle="Join the nation." /></div>
<main>
  <h1>All sorted.</h1>
  <!-- desired outcome = image instead of solid colour text box, adjust with screen -->
  <div class="row">
    <div class="column">
      <img src="chchStreet.JPG" alt="Christchurch Streets" />
      <div class="bottom-left">Easier than ever before.</div>
    </div>
    <div class="column"><img src="placeholder.png" alt="place holder" /></div>
    <div class="column"><img src="placeholder.png" alt="place holder" /></div>
  </div>
  <span class="textBox"><p>Simple and accurate.</p></span>
  <span class="textBox"><p>Work out your flat now.</p></span>
  <span class="textBox"><p>Made for students, by students.</p></span>
</main>
<Footer />

<style>
  .row {
    display: flex;
    flex-wrap: wrap;
    padding: 0 4px;
  }

  .column {
    flex: 25%;
    padding: 0 4px;
  }
  .column img {
    margin-top: 8px;
    vertical-align: middle;
    border-radius: 25%;
  }
  h1 {
    font-weight: 600;
    color: black;
    font-family: "Inter", sans-serif;
  }
  .textBox {
    height: 19vw;
    width: 19vw;
    border-radius: 25%;
    display: inline-flex;
    margin: 2vw;
    padding: 2vw;
    background-color: grey;
  }
  p {
    color: white;
    font-family: "Inter", sans-serif;
    justify-content: center;
    font-size: 2.5vw;
    font-weight: 600;
  }
</style>
