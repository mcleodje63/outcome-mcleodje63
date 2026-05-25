<script>
  import { onMount } from "svelte"
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"

  const maxFlatmates = 10

  let rent = 0
  let flatPop = 0

  let newRent = []
  let rentResponse = ""

  let people = []
  let rentPercent = []
  let utilities = []

  let showPercentages = false
  let showRentDetails = false

  let validationErrors = []

  let errorTimeout

  // -----------------------
  // Reactive calculations
  // -----------------------
  $: totalUtilities = utilities.reduce((acc, utility) => acc + (Number(utility.value) || 0), 0)

  $: overallTotal = Number(rent) + totalUtilities

  // -----------------------
  // Lifecycle
  // -----------------------
  onMount(() => {
    loadPeople()
    loadUtilities()
  })

  // Auto-clear error message
  $: if (rentResponse) {
    clearTimeout(errorTimeout)
    errorTimeout = setTimeout(() => {
      rentResponse = ""
    }, 5000)
  }

  // -----------------------
  // Validation
  // -----------------------
  function validateRent() {
    if (rent > 2000) return "Your number is too big"
    if (rent < 1) return "Your number is too small"
    if (isNaN(rent)) return "Invalid, please try again"
    return ""
  }

  function validateName(name, index) {
    if (!validationErrors[index]) validationErrors[index] = false

    if (!name || name.trim().length < 2) {
      validationErrors[index] = true
      rentResponse = "Each name must be at least 2 letters long."
    } else {
      validationErrors[index] = false
    }
  }

  // -----------------------
  // People logic
  // -----------------------
  function addPerson() {
    if (rent <= 0) {
      rentResponse = "Please enter the rent amount before adding people."
      return
    }

    if (flatPop >= maxFlatmates) {
      rentResponse = "You can only add up to 10 flatmates."
      return
    }

    people = [...people, ""]
    rentPercent = [...rentPercent, 0]
    validationErrors = [...validationErrors, false]

    flatPop += 1

    savePeople()
  }

  function removePerson(index) {
    people = people.filter((_, i) => i !== index)
    rentPercent = rentPercent.filter((_, i) => i !== index)
    validationErrors = validationErrors.filter((_, i) => i !== index)

    flatPop = Math.max(0, flatPop - 1)

    savePeople()
  }

  // -----------------------
  // UI toggles
  // -----------------------
  function splitCalculator() {
    showPercentages = !showPercentages
    validateRent()
  }

  function reset() {
    rent = 0
    people = []
    rentPercent = []
    utilities = []
    newRent = []
    flatPop = 0
    rentResponse = ""
    validationErrors = []
    showRentDetails = false
  }

  // -----------------------
  // Utilities (minimal working stubs)
  // -----------------------
  function addUtility() {
    utilities = [...utilities, { name: "", value: 0, saved: false, editing: true }]
  }

  function updateUtilityName(index, value) {
    utilities[index].name = value
  }

  function updateUtilityValue(index, value) {
    utilities[index].value = Number(value)
  }

  function saveUtility(index) {
    utilities[index].saved = true
    utilities[index].editing = false
  }

  function removeUtility(index) {
    utilities = utilities.filter((_, i) => i !== index)
  }

  // -----------------------
  // Persistence stubs
  // -----------------------
  function loadPeople() {
    // placeholder (replace with localStorage if you want)
  }

  function savePeople() {
    // placeholder (replace with localStorage if you want)
  }

  function loadUtilities() {
    // placeholder
  }

  // -----------------------
  // Main action
  // -----------------------
  function calculateRent() {
    validateRent()
    showRentDetails = true

    if (people.length === 0) {
      newRent = []
      return
    }

    const baseShare = rent / people.length

    newRent = people.map((name, i) => {
      const percent = rentPercent[i] || 0

      const amount = showPercentages ? (rent * percent) / 100 : baseShare

      return {
        name,
        amount: amount.toFixed(2),
      }
    })
  }
</script>

<Navigation />

<main>
  <span class="imgContainer">
    <img src="hillTree.JPG" style="max-width: 100%;" alt="tree on hill" />
  </span>

  <div class="header">
    <Header title="Start calculating" subtitle="Right now." />
  </div>

  <div class="calcFunction">
    <div class="calcHead">
      <p>Welcome to Flatter</p>
    </div>

    <p>How much rent does your flat pay per week?</p>

    <input class="userInput" type="number" bind:value={rent} min="1" max="2000" />

    <div class="buttonWrapper">
      <button class="addButton" on:click={addPerson}> Add person </button>

      <div class="switchWrapper">
        <span class="label-text">
          {showPercentages ? "Even Splitting" : "Uneven Splitting %"}
        </span>

        <label class="switch">
          <input type="checkbox" on:change={splitCalculator} />
          <span class="slider"></span>
        </label>
      </div>
    </div>

    {#each people as person, index}
      <div class="person">
        <input class="userInput" placeholder="Flatmate name" bind:value={people[index]} on:input={(e) => validateName(e.target.value, index)} class:error={validationErrors[index]} />

        {#if showPercentages}
          <input class="inputPercent" type="number" min="0" max="100" bind:value={rentPercent[index]} />
        {/if}

        <button class="removeButton" on:click={() => removePerson(index)}> 🗑 </button>
      </div>
    {/each}

    <div class="errorMessage">
      <b>{rentResponse}</b>
    </div>

    <button class="resetButton" on:click={reset}> Reset </button>

    <button class="addUtilitiesButton" on:click={addUtility}> Add Utilities </button>

    {#each utilities as utility, index}
      <div class="utility">
        <input class="utilityLabel" placeholder="Utility Name" bind:value={utility.name} on:input={(e) => updateUtilityName(index, e.target.value)} />

        <input class="utilityValue" type="number" min="0" max="500" bind:value={utility.value} on:input={(e) => updateUtilityValue(index, e.target.value)} />

        <button class="saveUtilityButton" on:click={() => saveUtility(index)}> Save </button>

        {#if utility.saved}
          <p>
            <strong>{utility.name}</strong>: ${utility.value.toFixed(2)}
          </p>

          <p>
            Split among {flatPop} people: ${(utility.value / (flatPop || 1)).toFixed(2)}
          </p>

          <button class="removeUtilityButton" on:click={() => removeUtility(index)}> 🗑 </button>
        {/if}
      </div>
    {/each}

    <button class="btn-hover" on:click={calculateRent}> Get rent </button>

    {#if showRentDetails}
      <div class="calcAnswers">
        <p>Your flat pays ${rent} a week</p>
        <p>
          There {flatPop === 1 ? "is" : "are"}
          {flatPop}
          {flatPop === 1 ? "person" : "people"} in your flat
        </p>

        {#if newRent.length > 0}
          {#each newRent as rentDetail}
            <p>{rentDetail.name} = ${rentDetail.amount}</p>
          {/each}
        {:else}
          <p>No rent details available</p>
        {/if}
      </div>
    {/if}
  </div>
</main>

<Footer />
