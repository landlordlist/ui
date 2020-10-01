<template>
    <header id="site-header" class="container">
        <article class="section">
            <router-link to="/"><img src="./../assets/logo.svg"></router-link>
        </article>
    </header>
    <section class="ll container strip">
        <article class="section banner">
            <p>
                <router-link to="/">&larr; Back</router-link>
            </p>
            <h1>House of {{ house.name }} </h1>
        </article>
    </section>

    <section id="balance" class="ll container">
        <article class="section">
            <div class="card">
                <header class="card-header">
                    <h2 class="card-title h3">Balance</h2>
                </header>
                <section class="card-body">
                    <section>
                        <p class="text-primary h5">Landlords ({{ landlordCount }})</p>
                        <div class="grid">
                            <div class="dot ll" v-for="i in landlordCount" v-bind:key="i"></div>
                        </div>
                    </section>
                    <section>
                        <p class="text-gray h5">Not landlords ({{ membersLessLandlords }})</p>
                        <div class="grid">
                            <div class="dot" v-for="i in membersLessLandlords" v-bind:key="i"></div>
                        </div>
                    </section>
                </section>
                <section class="card-footer">
                </section>
            </div>
        </article>
    </section>

    <main class="home ll container">
        <article class="section">
            <section id="members-list" :class="(isLoading ? 'loading loading-lg' : '')">
                <template v-for="(member, key) in members" v-bind:key="key">
                    <PersonBox :member="member"></PersonBox>
                </template>
            </section>
            <button class="btn btn-primary" @click="getMoreMembers()">More</button>
        </article>
    </main>
</template>


<script>
import PersonBox from '@/components/PersonBox.vue' 

export default {
    name: 'List',
    components: {
        PersonBox
    },
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
        },
        membersLessLandlords() {
            return this.memberCount - this.landlordCount
        },
        landlordCount() {
            if (this.house.counts != undefined) {
                return this.house.counts.landlords
            }

            return 0
        },
        memberCount() {
            if (this.house.counts != undefined) {
                return this.house.counts.members
            }

            return 0
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

    .card-body {
        display: flex;
        align-items: flex-start;

        section {
            flex: 1 1 auto;
        }
    }

    .h3 {
        margin: 0;
    }

    .grid {
        display: grid;
        grid-template-columns: repeat( auto-fit, minmax(8px, 1fr) );
        margin-right: 1rem;
        gap: 3px;
    }

    .dot {
        background-color: grey;
        min-height: 8px;
        &.ll {
            background-color: salmon;
        }
    }

    @media screen and (max-width: 650px) {
        .grid {
            grid-template-columns: repeat( auto-fit, minmax(4px, 1fr) );
            gap: 1px;
        }

        .dot {
            min-height: 4px;
        }
    }
}

#members-list {
    display: grid;
    grid-template-columns: repeat( auto-fit, minmax(300px, 1fr) );
    gap: 1rem;
    margin: 0 0 2rem 0;
}

.banner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: .88rem;
}

.strip {
    background-color: #fff;
    border-bottom: 1px solid darken(#fff, 10%)
}
</style>