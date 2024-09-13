<script>
  import Navigation from "$lib/Navigation.svelte"
  import Footer from "$lib/Footer.svelte"
</script>

<Navigation />

<main>
  <body>
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
      <div class="text-left slide-in">
        <h1>All Sorted.</h1>
        <hr />
        <p>All your stuff in one place, no more confusing spreadsheets or rushed group texts.</p>
      </div>
      <img src="centralCity.JPG" alt="Christchurch shops" id="christchurchShop" />
    </div>

    <div class="container-left">
      <img src="mountain.png" alt="mountain" id="mountain" />
      <div class="text-right slide-in">
        <h1>Loved by everyone.</h1>
        <p>Designed for university students in New Zealand, but anyone can use it.</p>
        <hr />
      </div>
    </div>
    <!-- use this structure for the larger images  --></body
  >
</main>
<Footer />

<style>
  body {
    margin: 0;
    font-family: "Inter", sans-serif;
    color: #333;
  }
  h1 {
    font-weight: 700;
    font-size: 3.5vw;
    color: black;
    margin-left: 2vw;
    padding-top: 5vw;
    width: 20vw;
  }
  p {
    line-height: 1.6;
  }
  hr {
    border: none;
    border-top: 1px solid #000;
    width: 40vw;
    margin-left: 2vw;
    margin-top: 3vw;
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
    overflow: hidden;
    border-radius: 10px;
    transition:
      transform 0.3s ease,
      box-shadow 0.3 ease;
  }
  .img-wrapper img {
    width: 30vw;
    height: 40vw;
    display: block;
    transition: transform 0.3s ease;
  }
  .img-wrapper:hover img {
    transform: scale(1.1);
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
    transition: transform 0.3s ease;
  }
  .container-left img {
    width: 55%;
  }
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
</style>
