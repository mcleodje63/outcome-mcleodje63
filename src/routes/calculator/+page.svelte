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
  let showRentDetails = false
  let utilities = []
  let totalUtilities = 0
  let overallTotal = 0
  let errorTimeout
  const maxFlatmates = 10

  onMount(() => {
    loadPeople()
    loadUtilities()
  })
  $: if (rentResponse) {
    clearTimeout(errorTimeout)
    errorTimeout = setTimeout(() => {
      rentResponse = ""
    }, 5000)
  }
  function fundCalculator() {
    if (rent > 2000) {
      rentResponse = "YOUR NUMBER IS TOO BIG"
      newRent = []
      return
    } else if (rent < 1) {
      // or if  the utilies isnt right?
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
    const totalUtilities = utilities.reduce((acc, utility) => acc + utility.value, 0)
    const overallTotal = rent + totalUtilities

    if (showPercentages) {
      let totalPercent = rentPercent.reduce((acc, percent) => acc + parseFloat(percent), 0)
      if (totalPercent !== 100) {
        rentResponse = "PERCENTAGES MUST ADD UP TO 100%"
        newRent = []
        return
      }

      newRent = people.map((person, index) => {
        const percent = parseFloat(rentPercent[index]) || 0
        const amount = parseFloat((overallTotal * (percent / 100)).toFixed(2))
        return {
          name: person || "Unnamed",
          percent: percent,
          amount: amount.toFixed(2),
        }
      })
    } else {
      if (flatPop > 0) {
        const amount = (overallTotal / flatPop).toFixed(2)
        newRent = [{ amount }]
      } else {
        newRent = []
        rentResponse = "Add people to split the rent."
      }
    }
    showRentDetails = true
    savePeople()
  }
  function splitCalculator() {
    showPercentages = !showPercentages
    fundCalculator()
  }

  function addPerson() {
    if (flatPop < maxFlatmates) {
      people = [...people, ""]
      rentPercent = [...rentPercent, 0]
      flatPop = flatPop + 1
      savePeople()
    } else {
      rentResponse = "You can only add up to 10 flatmates."
    }
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

  //utilies functions

  function addUtility() {
    utilities = [...utilities, { name: "", value: 0, saved: false }]
  }

  function saveUtility(index) {
    utilities[index].saved = true
    saveUtilities()

    totalUtilities = utilities.reduce((acc, utility) => acc + utility.value, 0)
    overallTotal = rent + totalUtilities
    if (newRent.length > 0) {
      if (flatPop > 0) {
        const utilityPerPerson = totalUtilities / flatPop
        newRent = newRent.map((rentDetail) => {
          return {
            ...rentDetail,
            amount: (parseFloat(rentDetail.amount) + utilityPerPerson).toFixed(2),
          }
        })
      } else {
        newRent = [{ amount: totalUtilities.toFixed(2) }]
      }
    } else {
      if (flatPop > 0) {
        const utilityPerPerson = totalUtilities / flatPop
        newRent = people.map(() => ({ amount: utilityPerPerson.toFixed(2) }))
      } else {
        newRent = [{ amount: totalUtilities.toFixed(2) }]
      }
    }
    updateTotals()
  }

  function updateUtilityName(index, name) {
    utilities[index].name = name
  }
  function updateUtilityValue(index, value) {
    utilities[index].value = parseFloat(value) || 0
    //change this? So theres not a random 0?
  }
  function removeUtility(index) {
    utilities = [...utilities.slice(0, index), ...utilities.slice(index + 1)]
    saveUtilities()

    updateTotals()
  }
  function saveUtilities() {
    localStorage.utilities = JSON.stringify(utilities)
  }

  function loadUtilities() {
    if (localStorage.utilities) {
      utilities = JSON.parse(localStorage.utilities)
    }
  }
  function updateTotals() {
    totalUtilities = utilities.reduce((acc, utility) => acc + utility.value, 0)
    overallTotal = rent + totalUtilities

    // Update the rent split calculation
    if (showPercentages) {
      let totalPercent = rentPercent.reduce((acc, percent) => acc + parseFloat(percent), 0)
      if (totalPercent !== 100) {
        rentResponse = "PERCENTAGES MUST ADD UP TO 100%"
        newRent = []
        return
      }

      newRent = people.map((person, index) => {
        const percent = parseFloat(rentPercent[index]) || 0
        const amount = parseFloat((overallTotal * (percent / 100)).toFixed(2))
        return {
          name: person || "Unnamed",
          percent: percent,
          amount: amount.toFixed(2),
        }
      })
    } else {
      if (flatPop > 0) {
        const amount = (overallTotal / flatPop).toFixed(2)
        newRent = [{ amount }]
      } else {
        newRent = []
        rentResponse = "Add people to split the rent."
      }
    }

    showRentDetails = true
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
    totalUtilities = 0
    overallTotal = 0
    showRentDetails = false
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

    <br />
    <div class="buttonWrapper">
      <button
        class="addButton"
        on:click={() => {
          addPerson()
          loadPeople()
        }}
      >
        Add person
      </button>
      <div class="switchWrapper">
        <span class="label-text">{showPercentages ? "Even Splitting" : "Uneven Splitting %"}</span>
        <label class="switch">
          <input type="checkbox" on:change={splitCalculator} />
          <span class="slider"></span>
        </label>
      </div>
    </div>
    <!-- make a slider -->

    {#each people as person, index}
      <div class="person">
        <input class="userInput" placeholder="Flatemates name" bind:value={people[index]} />

        {#if showPercentages}
          <input class="inputPercent" placeholder="%" type="number" bind:value={rentPercent[index]} min="0" max="100" />
          <!-- make this save too -->
        {/if}

        <button class="removeButton" on:click={() => removePerson(index)}>🗑</button>
      </div>
    {/each}

    <div class="errorMessage">
      <b>{rentResponse}</b>
    </div>

    <!-- create boundries -->

    <button class="resetButton" on:click={reset}>Reset</button>

    <button class="addUtilitiesButton" on:click={addUtility}>Add Utilities</button>

    {#each utilities as utility, index}
      <div class="utility">
        {#if !utility.saved || utility.editing}
          <input class="utilityLabel" placeholder="Utility Name" bind:value={utility.name} on:input={(e) => updateUtilityName(index, e.target.value)} />
          <input class="utilityValue" type="number" placeholder="Enter utility cost" min="0" max="500" bind:value={utility.value} on:input={(e) => updateUtilityValue(index, e.target.value)} />
          <button class="saveUtilityButton" on:click={() => saveUtility(index)}>Save</button>
          {#if utility.saved}{/if}
        {:else}
          <div class="utilitySaved">
            <p><strong>{utility.name}</strong> cost: ${utility.value.toFixed(2)}</p>
            {#if flatPop > 0}
              <!-- Split cost among people -->
              <p>Split {utility.name} among {flatPop} {flatPop === 1 ? "person" : "people"}: ${(utility.value / flatPop).toFixed(2)}</p>
            {:else}
              <p>Add people to split the {utility.name} cost.</p>
            {/if} <button class="removeUtilityButton" on:click={() => removeUtility(index)}>🗑</button>
          </div>
        {/if}
      </div>
    {/each}
    <button
      class="btn-hover"
      on:click={() => {
        fundCalculator()
        loadPeople()
      }}
    >
      Get rent</button
    >
    {#if showRentDetails}
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
    {/if}
  </div>
</main>

<Footer />

<style>
  .buttonWrapper {
    display: flex;
    align-items: center;
    gap: 8vw;
    margin: 1vw;
  }
  .switchWrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
  }

  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    transition: 0.4s;
    border-radius: 34px;
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    transition: 0.4s;
    border-radius: 50%;
  }

  input:checked + .slider {
    background-color: #2196f3;
  }

  input:checked + .slider:before {
    transform: translateX(26px);
  }
  .label-text {
    margin-bottom: 5px;
    font-size: 1vw;
  }
  .utilityLabel {
    width: 10vw;
  }
  .utilityValue {
    width: 8vw;
  }
  .addUtilitiesButton {
    border: 2px solid white;
  }
  .addUtilitiesButton:hover {
    background-color: white;
    color: #808080;
    border: 2px solid grey;
  }
  .removeUtilityButton {
    border-radius: 100%;
    width: 3.5vw;
    font-size: 1.5vw;
  }
  .addButton {
    border: 2px solid white;
    margin-top: 3vw;
  }
  .addButton:hover {
    background-color: white;
    color: #808080;
    border: 2px solid grey;
  }
  button {
    background-color: #c6bdbd;
    color: white;
    font-family: "Inter", sans-serif;
    border-radius: 10px;
    border-width: 0px;
    margin: 1vw;
    font-size: 1.8vw;
    padding: 1vw;
    text-align: center;
    font-weight: bold;
  }
  .removeButton {
    border-radius: 100%;
    width: 3.5vw;
    font-size: 1.5vw;
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
    border: 2px solid #407938;
  }
  .resetButton:hover {
    color: white;
    background-color: #407938;
    border: 2px solid #407938;
  }
</style>
