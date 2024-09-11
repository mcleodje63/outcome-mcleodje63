<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
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
  <span class="textRectangle"><p>No more stress over missed or late payments.</p></span>

  <Footer />
</main>

<style>
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
    height: 19vw;
    width: 19vw;
    background-color: #98ac9b;
    color: #e8f7dd;
    border-radius: 10%;
    display: inline-flex;
    margin: 2vw;
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
