<template>
  <header id="site-header" class="container">
    <article class="section">
        <router-link to="/"><img src="./../assets/logo.svg"></router-link>
    </article>
  </header>
  <section id="balance" class="ll container">
      <article class="section">
          <p>
            <router-link to="/">&larr; Back</router-link>
            </p>
      </article>
  </section>
  <main class="home ll container">
    <article class="section">
        <h1 class="text-primary">House of {{ house.name }} </h1>
        <section id="members-list" :class="(isLoading ? 'loading loading-lg' : '')">
            <template v-for="(member, key) in members" v-bind:key="key">
                <article class="card">
                    <div class="card-header">
                        <img :src="member.avatar_url">
                        <div>
                            <h5 class="card-title h5">{{member.name}}</h5>
                            <span class="card-subtitle text-gray">{{ member.from}}</span>
                        </div>
                    </div>
                    <div class="card-footer">
                        <span class="chip" :style="`background-color: #${member.party.colour}`">
                            {{ member.party.name }}
                        </span>
                    </div>
                </article>
            </template>
        </section>
        <button class="btn btn-primary" @click="getMoreMembers()">More</button>
    </article>
  </main>
</template>


<script>
export default {
    name: 'List',
    data() {
        return {
            house: {},
            members: [],
            isLoading: false
        }
    },
    computed: {
        membersOffset() {
            // SQL OFFSET is 0 indexed, .length is 1 indexed
            return this.members.length
        }
    },
    methods: {
        async getHouse() {
            let r = await fetch(`https://landlordlist.uk/api/houses/${this.$route.params.name}`)
            let data = await r.json()
            this.house = data
        },
        async getMembers() {
            this.isLoading = true
            let r = await fetch(`https://landlordlist.uk/api/people?house=${this.$route.params.name}`)
            let data = await r.json()
            this.members = data
            this.isLoading = false
        },
        async getMoreMembers() {
            this.isLoading = true
            let r = await fetch(`https://landlordlist.uk/api/people?house=${this.$route.params.name}&offset=${this.membersOffset}`)
            let data = await r.json()
            this.members = this.members.concat(data)
            this.isLoading = false
        }
    },
    beforeRouteUpdate (to, from, next) {
        this.getHouse()
        this.getMembers()
        next()
    },
    beforeMount() {
        this.getHouse()
        this.getMembers()
    }
}
</script>


<style lang="scss" scoped>
#site-header {

  img {
    max-height: 80px;
    width: min-content;
    margin: .44rem 0;
  }

  > .section {
    display: flex;
    justify-content: center;
    align-items: center;
  }

}

#balance {
    padding-top: .88rem;
}

#members-list {
    display: grid;
    grid-template-columns: repeat( auto-fit, minmax(300px, 1fr) );
    gap: 1rem;
    margin: 0 0 2rem 0;
}

img {
    object-fit: cover;
    height: 100px;
    margin: 0 .66rem .33rem 0;
    border-radius: 3px;
    border: 1px solid whitesmoke;
}

.card-header {
    display: flex;
}
</style>