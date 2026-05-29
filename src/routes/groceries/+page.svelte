<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"

  let products = []

  let newProduct = {
    name: "",
    url: "",
    currentPrice: "",
    pastPrice: ""
  }

  let targetDate = "20240101"
  let selectedIndex = 0

  $: selectedProduct = products[selectedIndex]
  $: totalCurrent = products.reduce((sum, product) => sum + toNumber(product.currentPrice), 0)
  $: totalPast = products.reduce((sum, product) => sum + toNumber(product.pastPrice), 0)
  $: comparableCount = products.filter((product) => toNumber(product.pastPrice) > 0).length
  $: totalChange = comparableCount > 0 ? totalCurrent - totalPast : 0
  $: totalChangePercent = comparableCount > 0 && totalPast > 0 ? (totalChange / totalPast) * 100 : 0

  function toNumber(value) {
    const number = Number.parseFloat(String(value).replace(/[^0-9.]/g, ""))
    return Number.isFinite(number) ? number : 0
  }

  function formatMoney(value) {
    return new Intl.NumberFormat("en-NZ", {
      style: "currency",
      currency: "NZD"
    }).format(value)
  }

  function priceChange(product) {
    if (!toNumber(product.pastPrice)) return null
    return toNumber(product.currentPrice) - toNumber(product.pastPrice)
  }

  function priceChangePercent(product) {
    if (!toNumber(product.pastPrice)) return null
    return (priceChange(product) / toNumber(product.pastPrice)) * 100
  }

  function addProduct() {
    if (!newProduct.name.trim()) return

    products = [
      ...products,
      {
        ...newProduct,
        name: newProduct.name.trim(),
        url: newProduct.url.trim(),
        currentPrice: newProduct.currentPrice || "0",
        pastPrice: newProduct.pastPrice || ""
      }
    ]

    selectedIndex = products.length - 1
    newProduct = { name: "", url: "", currentPrice: "", pastPrice: "" }
  }

  function removeProduct(index) {
    products = products.filter((_, productIndex) => productIndex !== index)
    selectedIndex = Math.max(0, Math.min(selectedIndex, products.length - 1))
  }

  function updateProduct(index, field, value) {
    products = products.map((product, productIndex) => {
      if (productIndex !== index) return product
      return { ...product, [field]: value }
    })
  }

  function waybackUrl(product) {
    if (!product?.url) return "https://web.archive.org/web/*/https://www.paknsave.co.nz/shop/*"
    return `https://web.archive.org/web/${targetDate}*/${product.url}`
  }
</script>

<Navigation />

<main>
  <section class="grocery-hero">
    <div class="page-shell grocery-hero-grid">
      <div>
        <p class="eyebrow">PAK'nSAVE price history</p>
        <h1>Compare today's shop with archived prices.</h1>
        <p>
          Add grocery items, open matching Wayback captures, then enter the old price you can verify from the archived page.
        </p>
      </div>
      <div class="summary-panel" aria-label="Shop summary">
        <span>Comparable items</span>
        <strong>{comparableCount} / {products.length}</strong>
        <span>Current basket</span>
        <strong>{formatMoney(totalCurrent)}</strong>
        <span>Historical basket</span>
        <strong>{comparableCount ? formatMoney(totalPast) : "Pending"}</strong>
        <span>Difference</span>
        <strong class:increase={totalChange > 0} class:decrease={totalChange < 0}>
          {comparableCount ? `${formatMoney(totalChange)} (${totalChangePercent.toFixed(1)}%)` : "Pending"}
        </strong>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="page-shell comparison-layout">
      <aside class="control-panel">
        <label>
          Wayback target date
          <input class="form-input" bind:value={targetDate} inputmode="numeric" maxlength="8" placeholder="YYYYMMDD" />
        </label>

        <div class="add-product">
          <h2>Add item</h2>
          <label>
            Product name
            <input class="form-input" bind:value={newProduct.name} placeholder="Milk 2L" />
          </label>
          <label>
            PAK'nSAVE product URL
            <input class="form-input" bind:value={newProduct.url} placeholder="https://www.paknsave.co.nz/shop/product/..." />
          </label>
          <label>
            Current price
            <input class="form-input" bind:value={newProduct.currentPrice} inputmode="decimal" placeholder="5.49" />
          </label>
          <label>
            Archived price
            <input class="form-input" bind:value={newProduct.pastPrice} inputmode="decimal" placeholder="Optional" />
          </label>
          <button type="button" on:click={addProduct}>Add to comparison</button>
        </div>
      </aside>

      <div class="comparison-panel">
        <div class="toolbar">
          <div>
            <p class="eyebrow">Basket comparison</p>
            <h2>Current vs archived prices</h2>
          </div>
          <a class="toolbar-link" href={waybackUrl(selectedProduct)} target="_blank" rel="noreferrer">Open Wayback for selected</a>
        </div>

        {#if products.length}
          <div class="product-table" role="table" aria-label="Grocery price comparison">
            <div class="table-row table-head" role="row">
              <span>Item</span>
              <span>Current</span>
              <span>Archived</span>
              <span>Change</span>
              <span>Wayback</span>
            </div>

            {#each products as product, index}
              <div class="table-row product-row" class:is-selected={selectedIndex === index} role="row">
                <span>
                  <strong>{product.name}</strong>
                  <small>{product.url ? "Product URL saved" : "No product URL yet"}</small>
                </span>
                <span>
                  <input value={product.currentPrice} inputmode="decimal" aria-label={`Current price for ${product.name}`} on:input={(event) => updateProduct(index, "currentPrice", event.currentTarget.value)} />
                </span>
                <span>
                  <input value={product.pastPrice} inputmode="decimal" aria-label={`Archived price for ${product.name}`} on:input={(event) => updateProduct(index, "pastPrice", event.currentTarget.value)} />
                </span>
                <span class:increase={priceChange(product) > 0} class:decrease={priceChange(product) < 0}>
                  {priceChange(product) === null ? "Pending" : `${formatMoney(priceChange(product))} (${priceChangePercent(product).toFixed(1)}%)`}
                </span>
                <span>
                  <a href={waybackUrl(product)} target="_blank" rel="noreferrer">Open captures</a>
                </span>
              </div>
              <div class="row-actions">
                {#if product.url}
                  <a href={product.url} target="_blank" rel="noreferrer">Open current page</a>
                {/if}
                <button type="button" on:click={() => (selectedIndex = index)}>{selectedIndex === index ? "Selected" : "Select"}</button>
                <button type="button" on:click={() => removeProduct(index)}>Remove</button>
              </div>
            {/each}
          </div>
        {:else}
          <div class="empty-state">
            <h3>No grocery items yet</h3>
            <p>Add an item on the left. Product URLs are optional, but they make the Wayback capture search more precise.</p>
          </div>
        {/if}
      </div>
    </div>
  </section>
</main>

<Footer />

<style>
  .grocery-hero {
    padding: clamp(58px, 8vw, 104px) 0 44px;
    background:
      linear-gradient(120deg, rgba(24, 54, 31, 0.9), rgba(49, 95, 60, 0.72)),
      url("/adOne.jpg") center / cover;
    color: white;
  }

  .grocery-hero-grid {
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(280px, 380px);
    gap: clamp(24px, 5vw, 60px);
    align-items: end;
  }

  .grocery-hero h1 {
    max-width: 760px;
    margin: 12px 0 18px;
    color: white;
    font-size: clamp(2.6rem, 7vw, 5.8rem);
    font-weight: 900;
    line-height: 0.95;
    letter-spacing: 0;
  }

  .grocery-hero p:not(.eyebrow) {
    max-width: 620px;
    color: rgba(255, 255, 255, 0.86);
    font-size: 1.12rem;
    line-height: 1.65;
  }

  .summary-panel,
  .control-panel,
  .comparison-panel {
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: var(--surface);
    box-shadow: var(--shadow);
  }

  .summary-panel {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 12px 18px;
    padding: 22px;
    color: var(--ink);
  }

  .summary-panel span {
    color: var(--muted);
    font-weight: 750;
  }

  .summary-panel strong {
    text-align: right;
  }

  .comparison-layout {
    display: grid;
    grid-template-columns: minmax(250px, 320px) minmax(0, 1fr);
    gap: 22px;
    align-items: start;
  }

  .control-panel,
  .comparison-panel {
    padding: 22px;
  }

  .control-panel label,
  .add-product label {
    display: grid;
    gap: 8px;
    color: var(--brand-dark);
    font-size: 0.9rem;
    font-weight: 800;
  }

  .add-product {
    display: grid;
    gap: 16px;
    margin-top: 24px;
    padding-top: 22px;
    border-top: 1px solid var(--line);
  }

  .add-product h2,
  .toolbar h2 {
    margin: 0;
    color: var(--brand-dark);
    font-size: 1.4rem;
    font-weight: 900;
  }

  button {
    min-height: 44px;
    border: 1px solid var(--brand);
    border-radius: var(--radius);
    background: var(--brand);
    color: white;
    font-weight: 850;
  }

  .toolbar-link {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 44px;
    padding: 0 16px;
    border: 1px solid var(--brand);
    border-radius: var(--radius);
    background: var(--brand);
    color: white;
    font-weight: 850;
    text-decoration: none;
  }

  button:disabled {
    cursor: not-allowed;
    opacity: 0.6;
  }

  .toolbar {
    display: flex;
    justify-content: space-between;
    gap: 18px;
    align-items: center;
    margin-bottom: 18px;
  }

  .product-table {
    overflow-x: auto;
  }

  .table-row {
    display: grid;
    grid-template-columns: minmax(210px, 1.35fr) minmax(100px, 0.6fr) minmax(110px, 0.7fr) minmax(150px, 0.8fr) minmax(120px, 0.7fr);
    gap: 14px;
    align-items: center;
    min-width: 820px;
    width: 100%;
    padding: 14px 12px;
    border: 0;
    border-bottom: 1px solid var(--line);
    border-radius: 0;
    background: transparent;
    color: var(--ink);
    text-align: left;
    transform: none;
  }

  .table-head {
    color: var(--muted);
    font-size: 0.78rem;
    font-weight: 900;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .product-row.is-selected {
    background: var(--surface-soft);
  }

  .product-row small {
    display: block;
    margin-top: 5px;
    color: var(--muted);
    font-size: 0.82rem;
    line-height: 1.35;
  }

  .product-row input {
    width: 100%;
    min-height: 38px;
    padding: 7px 9px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: white;
    color: var(--ink);
  }

  .product-row a,
  .row-actions a {
    color: var(--brand);
    font-weight: 850;
  }

  .row-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    min-width: 820px;
    padding: 8px 12px 16px;
    border-bottom: 1px solid var(--line);
  }

  .row-actions button,
  .row-actions a {
    display: inline-flex;
    align-items: center;
    min-height: 34px;
    padding: 0 10px;
    border: 1px solid var(--line);
    border-radius: var(--radius);
    background: white;
    color: var(--brand-dark);
    font-size: 0.82rem;
    font-weight: 850;
    text-decoration: none;
  }

  .increase {
    color: #a33a22;
  }

  .decrease {
    color: #217449;
  }

  .empty-state {
    padding: 34px;
    border: 1px dashed var(--line);
    border-radius: var(--radius);
    background: #fbfcfa;
  }

  .empty-state h3 {
    margin: 0 0 8px;
    color: var(--brand-dark);
    font-size: 1.25rem;
    font-weight: 900;
  }

  .empty-state p {
    margin: 0;
    color: var(--muted);
    line-height: 1.65;
  }

  @media (max-width: 900px) {
    .grocery-hero-grid,
    .comparison-layout {
      grid-template-columns: 1fr;
    }

    .toolbar {
      align-items: stretch;
      flex-direction: column;
    }
  }
</style>
