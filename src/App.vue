<template>
  <router-view/>
  <footer class="ll container bg-dark">
    <section class="section">
      <h5>LandlordList</h5>
      <p>We try to keep this information up to date, but it's worth checking Parliament's own information 
        as this may get out of date over time.</p>
      <p>
        <router-link to="/">Home</router-link> | <a href="https://github.com/landlordlist">Github</a>
      </p>
    </section>
  </footer>
</template>

<style lang="scss">
$primary-color: #f15a24;
@import "node_modules/spectre.css/src/spectre";
@import url('https://fonts.googleapis.com/css2?family=PT+Sans:ital,wght@0,400;0,700;1,400;1,700&display=swap');

body {
  background-color: whitesmoke;
  display:flex; 
  flex-direction:column;
  position: relative;
  min-height: 100vh;
}

#app {
  font-family: 'PT Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  flex-grow:1;
  display:flex; 
  flex-direction:column;

  > main {
    flex-grow: 1;
    margin: 1.66rem 0;
  }
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.ll.container {
  display: flex;
  justify-content: center;
}
.section {
  flex: 0 1 866px;
  padding: 0 .33rem;
}

footer {
  margin: 2rem 0 0;
  box-shadow: 0 0.25rem 1rem rgba(48,55,66,.15);
  padding: 1.33rem 0;
}
</style>
