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
  <span class="imgContainer"> <img src="hillTree.JPG" alt="tree on hill" /></span>

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
    <button on:click={fundCalculator}>Get rent</button>

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
    padding-bottom: 3rem;
    text-align: center;
    justify-content: center;
    border: 1px solid #ccc;
    padding: 1rem;
    margin: 5rem 25rem;
    font-size: 1.5em;
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
    border-bottom: 10px soild #ccc;
    justify-content: center;
  }
  .header {
    position: absolute;
    top: 20em;
    right: 16px;
  }
  button {
    color: white;
    border-radius: 30px;
    border-color: #407938;
    background-color: #407938;
    margin: 1.5em;
    width: 50%;
    height: 50%;
    padding: 0.5em;
    font-size: 1em;
    font-family: "Inter", sans-serif;
    font-weight: bold;
  }
</style>
