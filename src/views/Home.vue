<template>
  <header id="site-header" class="container">
    <section class="section">
      <img src="./../assets/logo.svg">
      <p>
        A list of landlords who are in elected positions in the UK.
      </p>
      <p>
        Why you ask? Because our economy and democracy is increasingly being run in the interests
        of landlords, owners, and developers - and to the detriment of the ordinary person. This is very bad. Click to read more.
      </p>
      <p class="link">
        <router-link to="/about">Read more &rarr;</router-link>
      </p>
    </section>
  </header>
  <!-- <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </div> -->
  <main class="home ll container">
    <BodyList></BodyList>
  </main>
</template>

<script>
// @ is an alias to /src
import BodyList from '@/components/BodyList.vue'

export default {
  name: 'Home',
  components: {
    BodyList
  }
}
</script>

<style lang="scss">
#site-header {
  background-color: #fff;
  border-bottom: 1px solid darken(#fff, 10%);
  display: flex;
  justify-content: center;
  padding: 1rem .4rem;
  text-align: center;

  p {
    text-align: left;
    color: rgba(0,0,0,.68);
    font-size: 1.22em;

    &.link {
      text-align: right;
    }
  }

  img {
    width: clamp(200px, 450px, 75vw);
    margin: 1.33rem 0 2rem;
  }
}
</style>
