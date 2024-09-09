<script>
  import { onMount } from "svelte"
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"

  let rent = 0
  let flatPop = 0
  let newRent = 0
  let rentResponse = ""
  let people = []
  let rentPercent = []
  let showPercentages = false
  let showUtilities = false
  let utilities = []

  onMount(() => {
    loadPeople()
    loadUtilities()
  })

  function fundCalculator() {
    if (rent > 2000) {
      rentResponse = "YOUR NUMBER IS TOO BIG"
      newRent = []
      return
    } else if (rent < 1) {
      rentResponse = "YOUR NUMBER IS TOO SMALL"
      newRent = []
      return
    } else if (isNaN(rent)) {
      rentResponse = "INVALID PLEASE TRY AGAIN"
      newRent = []
      return
    } else {
      rentResponse = ""
    }

    if (showPercentages) {
      let totalPercent = rentPercent.reduce((acc, percent) => acc + parseFloat(percent), 0)
      if (totalPercent !== 100) {
        rentResponse = "PERCENTAGES MUST ADD UP TO 100%"
        newRent = []
        return
      }

      newRent = people.map((person, index) => {
        const percent = parseFloat(rentPercent[index]) || 0
        const amount = parseFloat((rent * (percent / 100)).toFixed(2))
        return {
          name: person || "Unnamed",
          percent: percent,
          amount: amount.toFixed(2),
        }
      })
    } else {
      if (flatPop > 0) {
        const amount = (rent / flatPop).toFixed(2)
        newRent = [{ amount }]
      } else {
        newRent = []
        rentResponse = "Add people to split the rent."
      }
    }

    savePeople()
  }
  function splitCalculator() {
    showPercentages = !showPercentages
    fundCalculator()
  }

  function addPerson() {
    people = [...people, ""]
    rentPercent = [...rentPercent, 0]
    flatPop = flatPop + 1
    savePeople()
  }
  function removePerson(index) {
    people = [...people.slice(0, index), ...people.slice(index + 1)]
    rentPercent = [...rentPercent.slice(0, index), ...rentPercent.slice(index + 1)]
    if (flatPop > 0) {
      flatPop = flatPop - 1
    } else {
      flatPop = 0
    }
    savePeople()
  }
  function addUtility() {
    utilities = [...utilities, { name: "", value: 0, saved: false }]
  }

  function saveUtility(index) {
    utilities[index].saved = true
    saveUtilities()
  }

  function removeUtility(index) {
    utilities = [...utilities.slice(0, index), ...utilities.slice(index + 1)]
    saveUtilities()
  }
  function updateUtilityName(index, name) {
    utilities[index].name = name
  }
  function updateUtilityValue(index, value) {
    utilities[index].value = parseFloat(value) || 0
  }
  function saveUtilities() {
    localStorage.utilities = JSON.stringify(utilities)
  }

  function loadUtilities() {
    if (localStorage.utilities) {
      utilities = JSON.parse(localStorage.utilities)
    }
  }
  function savePeople() {
    localStorage.flatmates = JSON.stringify(people)
    localStorage.rentPercent = JSON.stringify(rentPercent)
    localStorage.flatRent = JSON.stringify(rent)
  }

  function loadPeople() {
    if (localStorage.flatmates) {
      people = JSON.parse(localStorage.flatmates)
      rentPercent = JSON.parse(localStorage.rentPercent)
      flatPop = people.length
      rent = JSON.parse(localStorage.flatRent) || 0
    }
  }
  function reset() {
    rent = 0
    flatPop = 0
    newRent = 0
    rentResponse = ""
    people = []
    rentPercent = []
    localStorage.clear()
    utilities = []
  }
</script>

<Navigation />
<head> <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css" /></head>
<main>
  <span class="imgContainer"> <img src="hillTree.JPG" style="max-width: 100%;" alt="tree on hill" /></span>

  <div class="header"><Header title="Start calculating" subtitle="Right now." /></div>
  <div class="calcFunction">
    <div class="calcHead">
      <p>Welcome to Flatter</p>
    </div>
    <p>How much rent does your flat pay per week?</p>
    <input class="userInput" type="number" bind:value={rent} min="1" max="2000" />
    <div class="field">
      <input type="checkbox" bind:checked={showUtilities} />
      <label>Press to add utilities</label>
    </div>
    {#if showUtilities}
      <button class="addUtilitiesButton" on:click={addUtility}>Add Utilities</button>{/if}

    <br />

    <p>Enter the people in your flat & % of rent</p>
    {#each people as person, index}
      <div class="person">
        <input class="userInput" bind:value={people[index]} />

        {#if showPercentages}
          <input class="inputPercent" type="number" bind:value={rentPercent[index]} min="0" max="100" />
          <!-- make this save too -->
        {/if}

        <button class="removeButton" on:click={() => removePerson(index)}>🗑</button>
      </div>
    {/each}

    <br />
    <button
      class="addButton"
      on:click={() => {
        addPerson()
        loadPeople()
      }}
    >
      Add person
    </button>
    <button
      class="btn-hover"
      on:click={() => {
        fundCalculator()
        loadPeople()
      }}
    >
      Get rent</button
    >
    <button class="btn-hover" on:click={splitCalculator}> {showPercentages ? "Even Splitting" : "Uneven Splitting"}</button>
    <div class="errorMessage">
      <b>{rentResponse}</b>
    </div>
    <div class="calcAnswers">
      <p>Your flat pays ${rent} a week</p>
      <p>There {flatPop === 1 ? "is" : "are"} {flatPop} {flatPop > 0 ? "person" : "people"} in your flat</p>
      {#if showPercentages}
        {#if newRent.length > 0}
          {#each newRent as rentDetail}
            <p>{rentDetail.name} = ${rentDetail.amount}</p>
          {/each}
        {:else}
          <p>No rent details available</p>
        {/if}
      {:else if newRent.length > 0}
        <p>Each person pays: ${newRent[0].amount}</p>
      {:else}<p>No rent details available</p>
      {/if}
    </div>
    <!-- create boundries -->

    <button class="resetButton" on:click={reset}>Reset</button>
  </div>
</main>

<Footer />

<style>
  .addUtilitiesButton {
    font-size: 1.8vw;
    padding: 1vw;
    text-align: center;
    border: 2px solid white;
  }
  .addUtilitiesButton:hover {
    background-color: white;
    color: #808080;
    border: 2px solid grey;
    padding: 1vw;
  }
  .addButton {
    font-size: 1.8vw;
    padding: 1vw;
    text-align: center;
    border: 2px solid white;
  }
  .addButton:hover {
    background-color: white;
    color: #808080;
    border: 2px solid grey;
    padding: 1vw;
  }
  button {
    background-color: #c6bdbd;
    color: white;
    font-family: "Inter", sans-serif;
    border-radius: 10px;
    border-width: 0px;
  }
  .removeButton {
    width: 3.5vw;
    height: 3.5vw;
    border-radius: 20px;
    font-size: 2.5vw;
  }
  .person {
    display: block;
    margin-left: 3.4vw;
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
    padding: 1vw;
    font-size: 1.5vw;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    text-align: center;
    margin: 1vw;
    border: 2px solid #407938;
  }
  .btn-hover:hover {
    background-color: white;
    color: black;
    border: 2px solid #407938;
    /* why is it moving? */
  }
  input {
    height: 2.5vw;
    width: 2.5vw;
    font-size: 1.25vw;
  }
  .userInput {
    height: 2.5vw;
    width: 15vw;
    margin: 0.5vw;
    font-family: "Inter", sans-serif;
    font-weight: 600;
    color: rgb(87, 87, 87);
    margin-left: auto;
    margin-right: auto;
  }
  .inputPercent {
    width: 5vw;
  }

  .errorMessage {
    color: white;
    font-size: 2vw;
    width: 28vw;
    margin-left: auto;
    margin-right: auto;
    background-color: red;
    border-radius: 10px;
    margin-top: 2vw;
    margin-bottom: 2vw;
  }
  .resetButton {
    color: black;
    background-color: white;
    width: 30%;
    height: 40%;
    padding: 1vw;
    font-size: 1vw;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    text-align: center;
    border: 2px solid #407938;
    margin: 1vw;
  }
  .resetButton:hover {
    color: white;
    background-color: #407938;
    width: 30%;
    height: 40%;
    padding: 1vw;
    font-size: 1vw;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    text-align: center;
    border: 2px solid #407938;
    margin: 1vw;
  }
</style>
