<script>
  import Navigation from "$lib/Navigation.svelte"
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
</script>

<Navigation />

<main>
  <h1>All sorted.</h1>
  <div class="img-container">
    <div class="img-wrapper">
      <img src="chchStreet.JPG" alt="" id="christchurchStreet" />
      <div class="overlay-text"><p>Do you know what you owe?</p></div>
    </div>
    <div class="img-wrapper">
      <img src="skytower.JPG" alt="Auckland-skytower-night" id="skytower" />
      <div class="overlay-text">
        <p>Squashing payments.</p>
      </div>
    </div>
    <div class="img-wrapper">
      <img src="concertMtSmart.jpg" alt="HSLOTMtSmart" id="mtSmart" />
      <div class="overlay-text">
        <p>More time to do what you love.</p>
      </div>
    </div>
  </div>

  <div class="container-right">
    <div class="text-left">
      <h1>All Sorted.</h1>
      <hr />
      <p>All your stuff in one place, no more confusing spreadsheets or rushed group texts.</p>
    </div>
    <img src="centralCity.JPG" alt="Christchurch shops" id="christchurchShop" />
  </div>

  <div class="container-left">
    <img src="mountain.png" alt="mountain" id="mountain" />
    <div class="text-right">
      <h1>Loved by everyone.</h1>
      <p>Designed for university students in New Zealand, but anyone can use it.</p>
      <hr />
    </div>
  </div>
  <!-- use this structure for the larger images  -->
</main>
<Footer />

<style>
  .text-left,
  .text-right {
    font-size: 2.5vw;
    font-weight: 300;
    width: 40%;
  }
  .text-left {
    display: flex;
    flex-direction: column;
  }
  .text-left p {
    margin-left: 3vw;
  }

  .text-right {
    display: flex;
    flex-direction: column;
  }
  .text-right p {
    margin-left: 2.2vw;
    margin-bottom: 3vw;
  }
  .img-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    position: relative;
  }
  .img-wrapper {
    position: relative;
    width: 30%;
    margin: 1%;
  }
  .img-wrapper img {
    width: 30vw;
    height: 40vw;
  }
  img {
    border-radius: 5%;
  }
  .container-right,
  .container-left {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 2vw;
    margin-top: 10vw;
    margin-bottom: 5vw;
  }

  .container-right img,
  .container-left img {
    width: 40%;
    height: auto;
    border-radius: 30px;
  }
  .container-left img {
    width: 55%;
  }

  h1 {
    font-weight: 700;
    font-size: 3.5vw;
    color: black;
    font-family: "Inter", sans-serif;
    margin-left: 2vw;
    padding-top: 5vw;
    width: 20vw;
  }

  p {
    color: 66eE6E;
    font-family: "Inter", sans-serif;
  }
  .overlay-text {
    position: absolute;
    top: 72%;
    bottom: 5%;
    left: 30%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 2.5vw;
    border-radius: 10px;
    font-weight: 700;
    margin-left: 3vw;
  }

  hr {
    border: none;
    border-top: 1px solid #000;
    width: 40vw;
    margin-left: 2vw;
    margin-top: 3vw;
  }
</style>
