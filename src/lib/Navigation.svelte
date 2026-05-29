<script>
  let navVisible = false

  function toggleNav() {
    // changes it from false to true, or true to false - just the opposite of what it currently is.
    navVisible = !navVisible
  }
</script>

<nav class="navbar">
  <div class="navbar-brand">
    <a class="navbar-item" href="/">
      <h1>Flatter</h1>
    </a>

    <button class="navbar-burger" class:is-active={navVisible} type="button" aria-label="Toggle navigation" aria-expanded={navVisible} on:click={toggleNav}>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </button>
  </div>

  <div class="navbar-menu" class:is-active={navVisible}>
    <!-- Add your navigation links here -->
    <div class="navbar-start">
      <!-- Navbar items on the left -->
      <a class="navbar-item" href="/learn">Learn</a> <a class="navbar-item" href="/calculator"> Calculator</a>
    </div>
  </div>
  <style>
    h1 {
      font-weight: bold;
    }
    a {
      margin-right: 2vw;
    }
    button {
      background: transparent;
      border: 0;
    }
  </style>
</nav>
