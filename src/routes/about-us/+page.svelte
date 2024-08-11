<script>
  import Navigation from "$lib/Navigation.svelte"
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
</script>

<Navigation />
<!-- <div class="header"><Header title="About us" subtitle="Join the nation." /></div> -->
<main>
  <h1>All sorted.</h1>
  <!-- desired outcome = image instead of solid colour text box, adjust with screen -->

  <div class="img-container">
    <img src="chchStreet.JPG" alt="" />
    <img src="chchStreet.JPG" alt="" />
    <img src="chchStreet.JPG" alt="" />
  </div>

  <div class="row">
    <div class="column">
      <div class="container">
        <div class="imageRow">
          <img src="chchStreet.JPG" alt="Christchurch Streets" />
          <div class="overlay-text">Say goodbye to late or forgotten bills</div>
        </div>
      </div>
      <div class="column"><img src="placeholder.png" alt="place holder" /></div>
      <div class="column"><img src="placeholder.png" alt="place holder" /></div>
    </div>
    <br />
    <h1>All Sorted</h1>
    <hr />
    <p>All your stuff in one place, no more confusing spreadsheets or rushed group texts.</p>

    <div class="imgRight"><img src="placeholder.png" alt="placeholder" /></div>

    <!-- <span class="textBox"><p>Simple and accurate.</p></span>
  <span class="textBox"><p>Work out your flat now.</p></span>
  <span class="textBox"><p>Made for students, by students.</p></span> -->
  </div>
</main>
<Footer />

<style>
  .img-container {
    display: flex;
    justify-content: space-around;
  }

  .img-container img {
    height: 200px;
  }

  .row {
    display: flex;
    flex-wrap: wrap;
    padding: 0 4px;
  }

  .column {
    flex: 25%;
    padding: 0 4px;
  }
  .container {
    position: relative;
    padding-bottom: 5vw;
    color: white;
  }

  .column img {
    margin-top: 8px;
    vertical-align: middle;
    border-radius: 10%;
  }
  img {
    height: 40vw;
    object-fit: fill;
    margin: 2vw;
  }
  h1 {
    font-weight: 650;
    font-size: 3.5vw;
    color: black;
    font-family: "Inter", sans-serif;
    margin-left: 3.5vw;
    padding-top: 5vw;
  }
  /* .textBox {
    height: 19vw;
    width: 19vw;
    border-radius: 25%;
    display: inline-flex;
    margin: 2vw;
    padding: 2vw;
    background-color: grey;
  } */
  p {
    color: 66eE6E;
    font-family: "Inter", sans-serif;
    font-size: 2.5vw;
    font-weight: 300;
    position: absalute;
    margin-left: 3.5vw;
    margin-right: 65vw;
  }
  .overlay-text {
    position: absolute;
    bottom: 8vw;
    left: 3vw;
    font-size: 2.6vw;
    font-weight: bold;
    color: white;
    font-family: "Inter", sans-serif;
    padding-right: 8vw;
  }
  hr {
    border: none;
    border-top: 1px solid #000;
    width: 40vw;
    margin-left: 3.5vw;
    margin-top: 3vw; /* change color */
  }
</style>
