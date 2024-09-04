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

  onMount(() => {
    loadPeople()
  })

  function fundCalculator() {
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
        newRent = people.map((person) => ({
          name: person || "Unnamed",
          percent: (100 / flatPop).toFixed(2),
          amount: (rent / flatPop).toFixed(2),
        }))
      } else {
        newRent = []
      }
    }

    if (rent > 2000) {
      rentResponse = "YOUR NUMBER IS TOO BIG"
      newRent = "N/A"
    } else if (rent < 1) {
      rentResponse = "YOUR NUMBER IS TOO SMALL"
      newRent = "N/A"
    } else if (isNaN(rent)) {
      rentResponse = "INVALID PLEASE TRY AGAIN"
      newRent = "N/A"
    } else {
      rentResponse = ""
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
  function savePeople() {
    localStorage.flatmates = JSON.stringify(people)
    localStorage.rentPercent = JSON.stringify(rentPercent)
  }

  function loadPeople() {
    if (localStorage.flatmates) {
      people = JSON.parse(localStorage.flatmates)
      rentPercent = JSON.parse(localStorage.rentPercent)
      flatPop = people.length
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
  }
</script>

<Navigation />
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
    <div class="errorMessage">
      <b>{rentResponse}</b>
    </div>
    <p>Enter the people in your flat & % of rent</p>
    {#each people as person, index}
      <div class="person">
        <input class="userInput" bind:value={people[index]} />

        {#if showPercentages}
          <input class="userInput" type="number" bind:value={rentPercent[index]} min="0" max="100" />
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
    <div class="calcAnswers">
      <p>Your flat pays ${rent} a week</p>
      <p>There {flatPop === 1 ? "is" : "are"} {flatPop} {flatPop > 0 ? "person" : "people"} in your flat</p>

      {#if newRent.length > 0}
        {#each newRent as rentDetail}
          <p>{rentDetail.name} = ${rentDetail.amount}</p>
        {/each}
      {:else}
        <p>No rent details available</p>
      {/if}
    </div>
    <!-- create boundries -->

    <button class="resetButton" on:click={reset}>Reset</button>
  </div>
</main>

<Footer />

<style>
  .addButton {
    font-size: 1.8vw;
    padding: 1vw;
    width: 12vw;
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
    font-size: 1vw;
  }
  .userInput {
    height: 2.5vw;
    width: 15vw;
    font-size: 1.25vw;
    margin: 0.5vw;
    font-family: "Inter", sans-serif;
    font-weight: 600;
    color: rgb(87, 87, 87);
    margin-left: auto;
    margin-right: auto;
  }

  .errorMessage {
    color: white;
    font-size: 2vw;
    width: 28vw;
    margin-left: 5vw;
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
</style>
