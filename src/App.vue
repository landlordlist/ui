<template>
  <router-view/>
  <footer class="ll container bg-primary">
    <section class="section">
      <h5>LandlordList</h5>
      <p>We try to keep this information up to date, but it's worth checking Parliament's own information 
        as this may get out of date over time.</p>
      <p class="flex">
        <router-link class="footer-link" to="/">Home</router-link> | <a class="footer-link" href="https://github.com/landlordlist">Github</a>
      </p>
    </section>
  </footer>
</template>

<style lang="scss">
$primary-color: #480bb2;
@import "node_modules/spectre.css/src/spectre";
@import "node_modules/spectre.css/src/spectre-icons";
@import url('https://fonts.googleapis.com/css2?family=PT+Sans:ital,wght@0,400;0,700;1,400;1,700&display=swap');

body {
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

  a {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: fit-content;

    i {
      margin: 0 .33rem;
    }
  }

  > main {
    flex-grow: 1;
    margin: 1.66rem 0;
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

  .footer-link {
    color: #fff;
  }
}

.flex {
  display: flex;
  > * {
    margin: 0 .33rem;

    &:first-child {
      margin-left: 0;
    }
  }
}
</style>
