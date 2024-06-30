<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  // should I have a sepertate page for the calculator? Or just have it as a pop-up
  // What about local storage?
  import Calculator from "$lib/Calculator.svelte"
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
<Header />

<main>
  <!-- <div class="columns"> -->
  <!-- big background image -->
  <!-- work out how to store stuff locally -->
  <p>How much rent does your flat pay per week?</p>
  <input bind:value={rent} />

  <p>How many people are in your flat?</p>
  <!-- change this to an drop down number (stop some valid inputs) -->
  <input bind:value={flatPop} />
  <br />
  <button on:click={fundCalculator}>Calculate new rent</button>

  <div class="calcAnswers">
    <p>Your flat pays {rent} a week</p>
    <p>There are {flatPop} people in your flat</p>
    <p>Your weekly total is {newRent}</p>
    <!-- create boundries -->
  </div>
</main>

<Footer />

<style>
  main {
    padding-bottom: 3rem;
    text-align: center;
    justify-content: center;
  }
  /* .columns {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    padding: 5rem 10rem;
  } */
</style>
