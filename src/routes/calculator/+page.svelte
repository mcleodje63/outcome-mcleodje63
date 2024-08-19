<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
  let rent = 0
  let flatPop = 0
  let newRent = 0
  let rentResponse = ""
  let people = []

  // if local stoarge people

  // let newRent = ...divided by how many members are in the flat
  function fundCalculator() {
    if (flatPop > 0) {
      newRent = rent / flatPop
      newRent = newRent.toFixed(2)
    } else {
      newRent = "N/A"
    }
    // Find method to lump, more effective
    if (rent > 2000) {
      rentResponse = "Your number is too big!"
      newRent = "N/A"
    } else if (rent < 1) {
      rentResponse = "Your number is too small!"
      newRent = "N/A"
    } else if (rent === e) {
      rentResponse = "Invalid, please try again"
      newRent = "N/A"
    } else {
      rentResponse = ""
    }
  }
  //save people + names, ability to: add, delete, edit, save load
  //save rent. Ability: edit, save, load.
  function addPerson() {
    people = [...people, ""]
    //add the simpler version
  }
  function removePerson(index) {
    people = [...people.slice(0, index), ...people.slice(index + 1)]
  }
  //combine save function with add person
  function savePeople() {
    localStorage.flatmates = JSON.stringify(people)
  }
  function loadPeople() {
    people = JSON.parse(localStorage.flatmates)
  }
</script>

<Navigation />
<main>
  <!-- add total mobile friendlyness  -->

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
    <input type="number" bind:value={rent} min="1" max="2000" />
    <br />
    <div class="errorMessage">
      <b>{rentResponse}</b>
    </div>
    <p>How many people are in your flat?</p>
    <!-- change this to an drop down number (stop some valid inputs) -->
    <select bind:value={flatPop} id="flatPopInput" min="1" max="10">
      <option value="1">1 person</option>
      <option value="2">2 people</option>
      <option value="3">3 people</option>
      <option value="4">4 people</option>
      <option value="5">5 people</option>
      <option value="6">6 people</option>
      <option value="7">7 people</option>
      <option value="8">8 people</option>
      <option value="9">9 people</option>
      <option value="10">10 people</option>
    </select><br />
    <button on:click={addPerson}>📝 Add </button>
    {#each people as person, index}
      <div class="person">
        <input bind:value={person} />
      </div>
      <button
        on:click={() => {
          removePerson(index)
        }}
        >🗑
      </button>
    {/each}
    <button on:click={savePeople}>💾</button>
    <button on:click={loadPeople}>📡 </button>
    <br />
    <button class="btn-hover color-1" on:click={fundCalculator}>Get rent</button>

    <div class="calcAnswers">
      <p>Your flat pays {rent} a week</p>
      <p>There {flatPop === 1 ? "is" : "are"} {flatPop} person{flatPop !== 1 ? "s" : ""} in your flat</p>
      <p>Your weekly total is {newRent}</p>
      <!-- create boundries -->
    </div>
  </div>
</main>

<Footer />

<style>
  .person {
    display: block;
  }

  .calcFunction {
    padding-bottom: 2vw;
    justify-content: center;
    border: 2px solid #ccc;
    border-radius: 10px;
    padding: 2vw;
    margin: 3vw 30vw;
    font-size: 1.5vw;
    text-align: center;
    position: relative;
    color: #6e6e6e;
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
    font-weight: 700;
    color: #6e6e6e;
    font-size: 2vw;
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
  .btn-hover {
    color: white;
    background-color: #407938;
    width: 40%;
    height: 50%;
    padding: 0.75vw;
    font-size: 1.5vw;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    text-align: center;
    border: 2px solid #407938;
    border-radius: 3vw;
    margin: 1vw;
  }
  .btn-hover:hover {
    background-color: white;
    color: black;
    border: 2px solid #407938;
  }
  input {
    height: 2.5vw;
    width: 15vw;
    font-size: 1.25vw;
    margin: 0.5vw;
    font-family: "Inter", sans-serif;
    font-weight: 600;
    color: rgb(87, 87, 87);
  }
  select {
    height: 2.5vw;
    width: 15vw;
    font-size: 1.25vw;
    /* add another font size for when the dropdown is clicked */
    margin: 0.5vw;
    font-family: "Inter", sans-serif;
    font-weight: 600;
    color: rgb(87, 87, 87);
  }
  .errorMessage {
    color: #407938;
    font-size: 2vw;
  }
</style>
