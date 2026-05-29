<script>
  import { onMount } from "svelte"
  import { browser } from "$app/environment"
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"

  const maxFlatmates = 10
  const storageKey = "flatter-calculator"

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
  let hasLoadedSavedState = false

  // -----------------------
  // Reactive calculations
  // -----------------------
  $: totalUtilities = utilities.reduce((acc, u) => acc + (Number(u.value) || 0), 0)

  $: overallTotal = Number(rent) + totalUtilities

  // -----------------------
  // Lifecycle
  // -----------------------
  onMount(() => {
    loadPeople()
    loadUtilities()
    hasLoadedSavedState = true
  })

  // Auto-clear message
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
    const rentAmount = Number(rent)

    if (Number.isNaN(rentAmount) || rent === "") return "Invalid, please try again"
    if (rentAmount > 2000) return "Your number is too big"
    if (rentAmount < 1) return "Your number is too small"
    return ""
  }

  function validatePeople() {
    if (people.length === 0) return "Please add at least one person."

    const nextValidationErrors = people.map((name) => !name || name.trim().length < 2)
    validationErrors = nextValidationErrors

    if (nextValidationErrors.some(Boolean)) {
      return "Each name must be at least 2 letters long."
    }

    return ""
  }

  function validatePercentages() {
    if (!showPercentages) return ""

    const percentages = rentPercent.map((percent) => Number(percent))
    const hasInvalidPercentage = percentages.some((percent) => Number.isNaN(percent) || percent < 0 || percent > 100)

    if (hasInvalidPercentage) return "Percentages must be between 0 and 100."

    const totalPercent = percentages.reduce((acc, percent) => acc + percent, 0)

    if (Math.abs(totalPercent - 100) > 0.01) {
      return "Uneven rent percentages must add up to 100%."
    }

    return ""
  }

  function validateName(name, index) {
    const isInvalid = !name || name.trim().length < 2

    validationErrors = validationErrors.map((v, i) => (i === index ? isInvalid : v))

    if (isInvalid) {
      rentResponse = "Each name must be at least 2 letters long."
    }
  }

  // -----------------------
  // People logic
  // -----------------------
  function addPerson() {
    if (validateRent()) {
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
  }

  function removePerson(index) {
    people = people.filter((_, i) => i !== index)
    rentPercent = rentPercent.filter((_, i) => i !== index)
    validationErrors = validationErrors.filter((_, i) => i !== index)

    flatPop = people.length
  }

  // -----------------------
  // UI toggles
  // -----------------------
  function splitCalculator(event) {
    showPercentages = event.currentTarget.checked
    rentResponse = validateRent() || validatePercentages()
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
    showPercentages = false
    saveState()
  }

  // -----------------------
  // Utilities
  // -----------------------
  function addUtility() {
    utilities = [...utilities, { name: "", value: 0, saved: false, editing: true }]
  }

  function updateUtilityName(index, value) {
    utilities = utilities.map((u, i) => (i === index ? { ...u, name: value } : u))
  }

  function updateUtilityValue(index, value) {
    utilities = utilities.map((u, i) => (i === index ? { ...u, value: Number(value) || 0 } : u))
  }

  function saveUtility(index) {
    const utility = utilities[index]

    if (!utility.name || utility.name.trim().length < 2) {
      rentResponse = "Utility names must be at least 2 letters long."
      return
    }

    if (Number(utility.value) < 0 || Number(utility.value) > 500) {
      rentResponse = "Utility amounts must be between $0 and $500."
      return
    }

    utilities = utilities.map((u, i) => (i === index ? { ...u, name: u.name.trim(), saved: true, editing: false } : u))
  }

  function removeUtility(index) {
    utilities = utilities.filter((_, i) => i !== index)
  }

  // -----------------------
  // Persistence
  // -----------------------
  function loadSavedState() {
    if (!browser) return {}

    const savedState = localStorage.getItem(storageKey)
    if (!savedState) return {}

    try {
      return JSON.parse(savedState)
    } catch {
      localStorage.removeItem(storageKey)
      return {}
    }
  }

  function loadPeople() {
    const savedState = loadSavedState()

    rent = Number(savedState.rent) || 0
    people = Array.isArray(savedState.people) ? savedState.people : []
    rentPercent = Array.isArray(savedState.rentPercent) ? savedState.rentPercent : people.map(() => 0)
    flatPop = people.length
    validationErrors = people.map(() => false)
    showPercentages = Boolean(savedState.showPercentages)
  }

  function loadUtilities() {
    const savedState = loadSavedState()
    utilities = Array.isArray(savedState.utilities) ? savedState.utilities : []
  }

  function saveState() {
    if (!browser || !hasLoadedSavedState) return

    localStorage.setItem(
      storageKey,
      JSON.stringify({
        rent,
        people,
        rentPercent,
        utilities,
        showPercentages,
      }),
    )
  }

  $: if (hasLoadedSavedState) {
    saveState()
  }

  // -----------------------
  // Main calculation
  // -----------------------
  function calculateRent() {
    const error = validateRent()
    const peopleError = validatePeople()
    const percentageError = validatePercentages()

    if (error || peopleError || percentageError) {
      rentResponse = error || peopleError || percentageError
      showRentDetails = false
      newRent = []
      return
    }

    showRentDetails = true

    const rentAmount = Number(rent)
    const baseShare = rentAmount / people.length
    const utilityShare = totalUtilities / people.length

    newRent = people.map((name, i) => {
      const percent = Number(rentPercent[i]) || 0
      const rentShare = showPercentages ? (rentAmount * percent) / 100 : baseShare
      const amount = rentShare + utilityShare

      return {
        name: name.trim(),
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
          <input type="checkbox" bind:checked={showPercentages} on:change={splitCalculator} />
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

          {#if flatPop > 0}
            <p>
              Split among {flatPop} people: ${(utility.value / flatPop).toFixed(2)}
            </p>
          {/if}

          <button class="removeUtilityButton" on:click={() => removeUtility(index)}> 🗑 </button>
        {/if}
      </div>
    {/each}

    <button class="btn-hover" on:click={calculateRent}> Get rent </button>

    {#if showRentDetails}
      <div class="calcAnswers">
        <p>Your flat pays ${rent} a week</p>
        {#if totalUtilities > 0}
          <p>Utilities add ${totalUtilities.toFixed(2)} a week</p>
          <p>Total weekly cost is ${overallTotal.toFixed(2)}</p>
        {/if}
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
