<!-- add mobile friendly hamburger.  -->
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="/home">
      <b>Flatter</b>
    </a>
    <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample" id="myNavbarBurger">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>
  <div id="navbarBasicExample" class="navbar-menu">
    <!-- Add your navigation links here -->
    <div class="navbar-start">
      <!-- Navbar items on the left -->
      <a href="/home" class="navbar-item">Home</a>
      <a href="/" class="navbar-item">Calculator</a>
      <!-- <a class="navbar-item">More</a> -->
    </div>
    <div class="navbar-end"></div>
    <div class="navbar-item has-dropdown is-hoverable">
      <a class="navbar-link">More</a>
      <div class="navbar-dropdown">
        <a href="/about" class="navbar-item">About</a>
        <a href="/contact" class="navbar-item">Contact</a>
      </div>

      <!-- Navbar items on the right -->
      <!-- <a class="navbar-item">Sign up</a>
      <a class="navbar-item">Log in</a> -->
    </div>
  </div>
</nav>
