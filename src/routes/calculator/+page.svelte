<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  // should I have a sepertate page for the calculator? Or just have it as a pop-up
  // What about local storage?
  // import Calculator from "$lib/Calculator.svelte"
  import Navigation from "$lib/Navigation.svelte"
  let rent = 0
  let flatPop = 0
  let newRent = 0
  // let newRent = ...divided by how many members are in the flat
  function fundCalculator() {
    if (flatPop > 0) {
      newRent = rent / flatPop
    } else {
      newRent = 0
    }
  }
</script>

<Navigation />
<main>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <span class="imgContainer"> <img src="hillTree.JPG" style="max-width: 100%;" alt="tree on hill" /></span>

  <div class="header"><Header title="Start calculating" subtitle="Right now." /></div>
  <!-- <div class="columns"> -->
  <!-- big background image -->
  <!-- work out how to store stuff locally -->

  <div class="calcFunction">
    <div class="calcHead">
      <p>Welcome to Flatter</p>
    </div>
    <p>How much rent does your flat pay per week?</p>
    <input bind:value={rent} />

    <p>How many people are in your flat?</p>
    <!-- change this to an drop down number (stop some valid inputs) -->
    <input bind:value={flatPop} />
    <br />
    <button class="btn-hover color-1" on:click={fundCalculator}>Get rent</button>

    <div class="calcAnswers">
      <p>Your flat pays {rent} a week</p>
      <p>There are {flatPop} people in your flat</p>
      <p>Your weekly total is {newRent}</p>
      <!-- create boundries -->
    </div>
  </div>
</main>

<Footer />

<style>
  .calcFunction {
    padding-bottom: 2vw;
    justify-content: center;
    border: 2px solid #ccc;
    padding: 2vw;
    margin: 3vw 25vw;
    font-size: 1.5vw;
    text-align: center;
    position: relative;
  }
  /* .columns {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    padding: 5rem 10rem;
  } */
  .imgContainer {
    position: relative;
    text-align: center;
    color: white;
    padding: 0;
  }
  .calcHead {
    border-bottom: 2px solid #ccc;
    justify-content: center;
    font-weight: bold;
  }
  .header {
    position: absolute;
    top: 25vw;
    right: 1vw;
  }
  * {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
  }
  button {
    color: white;
    background-color: #407938;
    width: 50%;
    height: 50%;
    padding: 0.5em;
    font-size: 1em;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    text-align: center;
    border: none;
    border-radius: 30px;
    margin: 1em;
  }
  input {
    height: 2.5em;
    width: 15em;
    font-size: 0.7em;
    margin: 1em;
  }
  /* button:hover {
    color: white;
    margin: 1.5rem;
    width: 50%;
    height: 50%;
    padding: 0.5em;
    font-size: 1em;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    border-radius: 30px;
    cursor: pointer;
    text-align: center;
    border: none;
    background-size: 300% 100%;
    moz-transition: all 0.4s ease-in-out;
    -o-transition: all 0.4s ease-in-out;
    -webkit-transition: all 0.4s ease-in-out;
    transition: all 0.4s ease-in-out;
  }

  .btn-hover:hover {
    background-position: 100% 0;
    moz-transition: all 0.4s ease-in-out;
    -o-transition: all 0.4s ease-in-out;
    -webkit-transition: all 0.4s ease-in-out;
    transition: all 0.4s ease-in-out;
  }

  .btn-hover:focus {
    outline: none;
  }
  .btn-hover.color-1 {
    background-image: linear-gradient(to right, #0ba360, #3cba92, #30dd8a, #2bb673);
    box-shadow: 0 4px 15px 0 rgba(23, 168, 108, 0.75);
  } */
</style>
