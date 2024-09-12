<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
  let currentIndex = 0
  const images = [
    { src: "house.JPG", alt: "Foggy house in Christchurch" },
    { src: "centralCity.JPG", alt: "Central City Image 1" },
  ]
</script>

<main>
  <Navigation />
  <span class="container">
    <img src="citySkyline.JPG" style="max-width: 100%;" alt="Singapore buildings" />
  </span>
  <div class="header">
    <Header title="Welcome to Flatter" subtitle="Rent made easy." /><a href="/calculator"> <button class="calculatorButton">The calculator</button></a>
  </div>

  <span class="textBox"><p>Easier than ever before.</p></span>
  <span class="textBox"><p>Simple and accurate.</p></span>
  <span class="textBox"><p>Work out your flat now.</p></span>
  <span class="textBox"><p>Made for students, by students.</p></span>
  <div class="container-right">
    <img class="mainImage" src="house.JPG" alt="Foggy house in Christchurch" />
    <img class="fanImage" src="centralCity.JPG" alt="Hidden Image 1" />
    <img class="fanImage" src="centralCity.JPG" alt="Hidden Image 2" />
  </div>
  <span class="textRectangle"><p>No more stress over missed or late payments.</p></span> <span class="reviewBox"><p>Hello there</p></span>
  <span class="reviewBox"><p>Hello there</p></span><span class="reviewBox"><p>Hello there</p></span>

  <Footer />
</main>

<style>
  .container-right {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 2vw;
    margin-top: 10vw;
    margin-bottom: 5vw;
    position: relative;
  }
  .container-right img {
    width: 30%;
    height: auto;
    border-radius: 30px;
  }
  .mainImage {
    width: 30%;
    height: auto;
    border-radius: 30px;
    transition: transform 0.6s ease;
    z-index: 10;
  }

  .mainImage:hover {
    transform: scale(1.03);
  }
  .fanImage {
    position: absolute;
    width: 5vw;
    height: auto;
    border-radius: 20px;
    opacity: 0;
    transition:
      transform 0.6s ease,
      opacity 0.6s ease;
  }

  .fanImage:nth-child(2) {
    transform: translateX(0px) rotate(0deg);
  }
  .fanImage:nth-child(3) {
    transform: translateX(0px) rotate(0deg);
  }

  .container-right:hover .fanImage:nth-child(2) {
    transform: translateX(-100px) rotate(-10deg);
    opacity: 1;
  }
  .container-right:hover .fanImage:nth-child(3) {
    transform: translateX(100px) rotate(10deg);
    opacity: 1;
  }

  .container {
    position: relative;
    text-align: center;
    color: white;
    padding: 0;
  }

  .header {
    position: absolute;
    top: 12.5vw;
    left: 1vw;
  }
  .calculatorButton {
    color: black;
    background-color: transparent;
    width: 14vw;
    height: 3.5vw;
    font-size: 1.5vw;
    font-family: "Inter", sans-serif;
    font-weight: 700;
    margin: 3.5vw;
    border: 0.4vw solid #407938;
    border-radius: 20px;
    position: absolute;
    top: 14vw;
    left: 1.15vh;
  }
  .calculatorButton:hover {
    background-color: #407938;
    color: white;
    border: 2px solid #407938;
    /* why is it moving? */
  }

  .textBox {
    height: 18vw;
    width: 18vw;
    background-color: #98ac9b;
    color: #e8f7dd;
    border-radius: 10%;
    display: inline-flex;
    margin: 2.5vw;
    margin-left: 3vw;
    padding: 2vw;

    transition:
      transform 0.3s ease,
      background-color 0.3s ease,
      border-color 0.3s ease,
      border-width 0.3s ease;
  }
  .textBox:hover {
    background: #748377;
    transform: translateY(-10px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
  } /* use this! it turns it to the side transform: rotate(5deg) translateY(-10px); */
  .reviewBox {
    height: 15vw;
    width: 25vw;
    background-color: #aac0ad;
    color: #6d7667;
    border-radius: 10px;
    display: inline-flex;
    margin: 2.5vw;
    margin-left: 3vw;
    padding: 2vw;

    transition:
      transform 0.3s ease,
      background-color 0.3s ease,
      border-color 0.3s ease,
      border-width 0.3s ease;
  }
  .reviewBox:hover {
    background: #748377;
    color: #e8f7dd;
    transform: translateY(-10px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
  }

  .textRectangle {
    height: 7vw;
    width: 90vw;
    background-color: #1e3d1a;
    color: #c1ceb8;
    border-radius: 10px;
    display: inline-flex;
    margin: 2vw;
    margin-left: 4.5%;
    padding: 1.7vw;
    text-align: center;
    justify-content: center;
    transition:
      background-color 0.3s ease,
      transform 0.3s ease;
  }

  .textRectangle:hover {
    background-color: #1e3d1a;
    transform: translateX(10px);
  }
  p {
    font-family: "Inter", sans-serif;
    justify-content: center;
    font-size: 2.5vw;
    font-weight: 600;
  }
</style>
