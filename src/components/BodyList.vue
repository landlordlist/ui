<template>
  <section :class="sectionClass">
    <template v-for="(house, index) in houses" v-bind:key="index">
      <div class="card">
        <div class="card-header">
          <div class="card-title h5">House of {{ house.name }}</div>
          <div class="card-subtitle text-gray">{{ house.members }} members</div>
        </div>
        <div class="card-footer">
          <router-link :to="getLink(house)" class="btn btn-primary">View Members</router-link>
        </div>
      </div>
    </template> 
  </section>
</template>

<script>
export default {
  name: 'BodyList',
  props: {},
  data() {
    return {
      houses: [],
      isLoading: false
    }
  },
  computed: {
    sectionClass() {
      let sectionClass = "section"

      if (this.isLoading) {
        sectionClass += " loading loading-lg"
      }

      return sectionClass
    }
  },
  methods: {
    async getHouses() {
      this.isLoading = true
      let r = await fetch('https://landlordlist.uk/api/houses')
      let data = await r.json()
      this.houses = data;
      this.isLoading = false;
    },
    getLink(house) {
      return `/list/${house.name}`
    }
  },
  beforeMount() {
    this.getHouses()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
section {
  align-items: start;
  display: grid;
  grid-template-columns: repeat( auto-fit, minmax(250px, 1fr) );
  gap: 1rem;
}
</style>
