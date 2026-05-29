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
    rent
    people
    rentPercent
    utilities
    showPercentages
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
  <section class="calculator-hero">
    <img src="/hillTree.JPG" alt="Tree on a hill" />
    <div class="hero-content">
      <Header title="Start calculating" subtitle="Autosaves as you type." eyebrow="Calculator" />
    </div>
  </section>

  <section class="section">
    <div class="page-shell calculator-layout">
      <div class="calculator-card">
        <div class="panel-heading">
          <div>
            <p class="eyebrow">Weekly rent</p>
            <h2>Flat details</h2>
          </div>
          <span class="autosave-badge">Autosaved</span>
        </div>

        <label class="field-group">
          <span>How much rent does your flat pay per week?</span>
          <input class="form-input" type="number" bind:value={rent} min="1" max="2000" placeholder="0.00" />
        </label>

        <div class="actions-row">
          <button class="primary-action" type="button" on:click={addPerson}>Add person</button>

          <label class="toggle-control">
            <span>{showPercentages ? "Uneven percentage split" : "Even split"}</span>
            <input type="checkbox" bind:checked={showPercentages} on:change={splitCalculator} />
          </label>
        </div>

        <div class="people-list">
          {#each people as person, index}
            <div class="person-row">
              <label>
                <span>Flatmate {index + 1}</span>
                <input class="form-input" placeholder="Flatmate name" bind:value={people[index]} on:input={(e) => validateName(e.target.value, index)} class:error={validationErrors[index]} />
              </label>

              {#if showPercentages}
                <label class="percent-field">
                  <span>Percent</span>
                  <input class="form-input" type="number" min="0" max="100" bind:value={rentPercent[index]} />
                </label>
              {/if}

              <button class="ghost-action" type="button" on:click={() => removePerson(index)}>Remove</button>
            </div>
          {/each}
        </div>

        {#if rentResponse}
          <div class="errorMessage" role="status">
            <b>{rentResponse}</b>
          </div>
        {/if}
      </div>

      <div class="calculator-card">
        <div class="panel-heading">
          <div>
            <p class="eyebrow">Bills</p>
            <h2>Utilities</h2>
          </div>
          <button class="secondary-action" type="button" on:click={addUtility}>Add utility</button>
        </div>

        {#if utilities.length === 0}
          <p class="empty-state">Add power, internet, water, or any shared weekly cost.</p>
        {/if}

        <div class="utility-list">
          {#each utilities as utility, index}
            <div class="utility">
              <input class="form-input" placeholder="Utility name" bind:value={utility.name} on:input={(e) => updateUtilityName(index, e.target.value)} />

              <input class="form-input" type="number" min="0" max="500" bind:value={utility.value} on:input={(e) => updateUtilityValue(index, e.target.value)} />

              <div class="utility-actions">
                <button class="secondary-action" type="button" on:click={() => saveUtility(index)}>Save</button>
                <button class="ghost-action" type="button" on:click={() => removeUtility(index)}>Remove</button>
              </div>

              {#if utility.saved}
                <p class="utility-summary">
                  <strong>{utility.name}</strong>: ${utility.value.toFixed(2)}
                  {#if flatPop > 0}
                    <span> ${(utility.value / flatPop).toFixed(2)} each</span>
                  {/if}
                </p>
              {/if}
            </div>
          {/each}
        </div>
      </div>

      <aside class="summary-card">
        <p class="eyebrow">Results</p>
        <h2>Weekly split</h2>
        <div class="summary-metric">
          <span>Total weekly cost</span>
          <strong>${overallTotal.toFixed(2)}</strong>
        </div>
        <div class="summary-metric">
          <span>Flatmates</span>
          <strong>{flatPop}</strong>
        </div>

        <button class="primary-action wide" type="button" on:click={calculateRent}>Get rent</button>
        <button class="ghost-action wide" type="button" on:click={reset}>Reset</button>

        {#if showRentDetails}
          <div class="calcAnswers">
            <p>Your flat pays ${rent} a week</p>
            {#if totalUtilities > 0}
              <p>Utilities add ${totalUtilities.toFixed(2)} a week</p>
            {/if}

            {#if newRent.length > 0}
              {#each newRent as rentDetail}
                <div class="result-row">
                  <span>{rentDetail.name}</span>
                  <strong>${rentDetail.amount}</strong>
                </div>
              {/each}
            {:else}
              <p>No rent details available</p>
            {/if}
          </div>
        {/if}
      </aside>
    </div>
  </section>
</main>

<Footer />

<style>
  .calculator-hero {
    position: relative;
    min-height: clamp(340px, 48vh, 520px);
    display: flex;
    align-items: flex-end;
    overflow: hidden;
    background: var(--brand-dark);
  }

  .calculator-hero > img {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .calculator-hero::after {
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, rgba(15, 27, 17, 0.84), rgba(15, 27, 17, 0.28));
  }

  .calculator-layout {
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(300px, 0.45fr);
    gap: 20px;
    align-items: start;
  }

  .calculator-card,
  .summary-card {
    padding: clamp(20px, 3vw, 30px);
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: var(--surface);
    box-shadow: 0 18px 45px rgba(35, 53, 38, 0.08);
  }

  .calculator-card {
    grid-column: 1;
  }

  .summary-card {
    position: sticky;
    top: 92px;
    grid-column: 2;
    grid-row: 1 / span 2;
  }

  .panel-heading {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    margin-bottom: 22px;
  }

  h2 {
    margin: 0;
    color: var(--brand-dark);
    font-size: clamp(1.45rem, 3vw, 2.2rem);
    font-weight: 900;
  }

  .field-group,
  .person-row label,
  .percent-field {
    display: grid;
    gap: 8px;
    color: var(--muted);
    font-size: 0.9rem;
    font-weight: 750;
  }

  .actions-row,
  .person-row,
  .utility {
    display: grid;
    gap: 12px;
  }

  .actions-row {
    grid-template-columns: auto 1fr;
    align-items: center;
    margin: 18px 0;
  }

  .people-list,
  .utility-list {
    display: grid;
    gap: 12px;
  }

  .person-row {
    grid-template-columns: minmax(0, 1fr) auto auto;
    align-items: end;
    padding: 14px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: #fbfcfa;
  }

  .percent-field {
    width: 120px;
  }

  .utility {
    padding: 14px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: #fbfcfa;
  }

  .utility-actions {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }

  .utility-summary,
  .empty-state,
  .calcAnswers p {
    margin: 0;
    color: var(--muted);
    line-height: 1.6;
  }

  .utility-summary span {
    color: var(--brand);
    font-weight: 800;
  }

  .primary-action,
  .secondary-action,
  .ghost-action {
    min-height: 44px;
    padding: 0 16px;
    border-radius: var(--radius);
    font-weight: 850;
  }

  .primary-action {
    border: 1px solid var(--brand);
    background: var(--brand);
    color: white;
    box-shadow: 0 12px 30px rgba(49, 95, 60, 0.2);
  }

  .secondary-action {
    border: 1px solid var(--line);
    background: var(--surface-soft);
    color: var(--brand-dark);
  }

  .ghost-action {
    border: 1px solid transparent;
    background: transparent;
    color: var(--muted);
  }

  .wide {
    width: 100%;
    margin-top: 12px;
  }

  .autosave-badge {
    padding: 6px 10px;
    border-radius: 999px;
    background: var(--surface-soft);
    color: var(--brand);
    font-size: 0.8rem;
    font-weight: 850;
  }

  .toggle-control {
    justify-self: end;
    display: inline-flex;
    align-items: center;
    gap: 10px;
    color: var(--muted);
    font-weight: 800;
  }

  .toggle-control input {
    width: 46px;
    height: 24px;
    accent-color: var(--brand);
  }

  .errorMessage {
    margin-top: 16px;
    padding: 12px 14px;
    border: 1px solid rgba(151, 56, 42, 0.2);
    border-radius: var(--radius);
    background: #fff0ec;
    color: #8d3124;
  }

  .error {
    border-color: #b94b3d;
    background: #fff7f5;
  }

  .summary-metric {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 14px;
    padding: 14px 0;
    border-bottom: 1px solid var(--line);
  }

  .summary-metric span {
    color: var(--muted);
    font-weight: 750;
  }

  .summary-metric strong {
    color: var(--brand-dark);
    font-size: 1.6rem;
    font-weight: 900;
  }

  .calcAnswers {
    display: grid;
    gap: 10px;
    margin-top: 18px;
  }

  .result-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 14px;
    padding: 12px 0;
    border-top: 1px solid var(--line);
  }

  .result-row strong {
    color: var(--brand);
    font-size: 1.15rem;
  }

  @media (max-width: 920px) {
    .calculator-layout {
      grid-template-columns: 1fr;
    }

    .calculator-card,
    .summary-card {
      grid-column: auto;
    }

    .summary-card {
      position: static;
      grid-row: auto;
    }
  }

  @media (max-width: 680px) {
    .actions-row,
    .person-row {
      grid-template-columns: 1fr;
    }

    .toggle-control {
      justify-self: start;
    }

    .percent-field {
      width: 100%;
    }
  }
</style>
