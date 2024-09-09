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
    <div class="favicon">
      <link rel="shortcut icon" type="image/png" href="flatterLogo" />
    </div>

    <Header title="Welcome to Flatter" subtitle="Rent made easy" />
  </div>

  <span class="textBox"><p>Easier than ever before.</p></span>
  <span class="textBox"><p>Simple and accurate.</p></span>
  <span class="textBox"><p>Work out your flat now.</p></span>
  <span class="textBox"><p>Made for students, by students.</p></span>

  <!-- <span class="textRectangle"><p>No more stress over missed or late payments.</p></span> <button class="calculatorButton" on:click={placeHolder}>The calculator</button> -->
  <Footer />
</main>

<style>
  .calculatorButton {
    color: black;
    background-color: white;
    width: 15%;
    height: 50%;
    padding: 1vw;
    font-size: 1.5vw;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    margin: 1vw;
    border: 2px solid #407938;
    border-radius: 15px;
    margin-left: 40%;
  }
  .calculatorButton:hover {
    background-color: #407938;
    color: white;
    border: 2px solid #407938;
    /* why is it moving? */
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
  .textBox {
    height: 19vw;
    width: 19vw;
    background-color: #e8d47c;
    border-radius: 10%;
    display: inline-flex;
    margin: 2vw;
    padding: 2vw;
  }
  .textBox:hover {
    background: rgb(231, 218, 158);
  }
  .textRectangle {
    height: 7vw;
    width: 70vw;
    background-color: #d26f38;
    border-radius: 10px;
    display: inline-flex;
    margin: 2vw;
    margin-left: 15%;
    padding: 1.7vw;
    text-align: center;
    justify-content: center;
  }

  .textRectangle:hover {
    background: #d48255;
  }
  p {
    color: white;
    font-family: "Inter", sans-serif;
    justify-content: center;
    font-size: 2.5vw;
    font-weight: 600;
  }
  .favicon {
    object-fit: cover;
    width: 1vw;
    height: 1vw;
  }
</style>
