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
                <ul class="tab">
                    <li :class="`tab-item ${isOverall ? 'active' : ''}`">
                        <a @click.prevent="isOverall=true" href="#">Overall</a>
                    </li>
                    <li :class="`tab-item ${isOverall ? '' : 'active'}`">
                        <a @click.prevent="isOverall=false" href="#" class="">By Party</a>
                    </li>
                </ul>
            </header>
            <section class="card-body" v-if="isOverall">
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
            <section class="card-body" v-if="!isOverall">
                <template v-for="(party, i) in parties" v-bind:key="i">
                    <section :style="`flex-basis: ${(party.members/memberCount)*100}%`">
                        <p class="h5">{{ party.abbr || 'None' }}</p>
                        <div class="grid">
                            <div class="dot ll" v-for="i in party.landlords" v-bind:key="i" :style="`background-color: #${party.colour}`"></div>
                            <div class="dot" v-for="i in (party.members-party.landlords)" v-bind:key="i" :style="`border-color: #${party.colour}`"></div>
                        </div>
                    </section>
                </template>
            </section>
            <div class="card-footer">
                <ul>
                    <li>
                        <div class="dot ll"></div> Landlord
                    </li>
                    <li>
                        <div class="dot"></div> Not a landlord
                    </li>
                </ul>
            </div>
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
            isLoading: false,
            isOverall: false
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
        },
        parties() {
            let output_array = []
            let other = {
                "_id": 0,
                "abbr": "Other",
                "colour": "C0C0C0",
                "landlords": 0,
                "members": 0,
                "name": "Other"
            }

            if (this.house.party_balance == undefined) {
                return []
            }

            for (var i = 0; i < this.house.party_balance.length; i++) {
                let party = this.house.party_balance[i]
                if (party.members > 10) {
                    output_array.push(party)
                } else {
                    other.landlords += party.landlords
                    other.members += party.members
                }
            }

            output_array.push(other)

            return output_array
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
    beforeRouteUpdate(to, from, next) {
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

    >.section {
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

    .h5 {
        margin: 0;
        padding-right: .33rem;
    }

    .grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(8px, 1fr));
        margin-right: .22rem;
        gap: 3px;
    }

    .dot {
        border: 2px solid #bcc3ce;
        background-color: #fff;
        height: 8px;
        width: 8px;
        border-radius: 100%;

        &.ll {
            background-color: salmon;
            border-color: transparent;
        }
    }

    .card-footer ul {
        display: flex;
        align-items: center;
        list-style-type: none;
        margin: 0;
        padding: 0;

        li {
            color: #a5a9b0;
            display: flex;
            align-items: center;
            margin: 0;
            padding: 0 .44rem 0 0;

            div.dot {
                margin: 0 .33rem 0 0;
                border-color: #a5a9b0;

                &.ll {
                    background-color: #a5a9b0;
                }
            }
        }
    }

    @media screen and (max-width: 650px) {
        .grid {
            grid-template-columns: repeat(auto-fit, minmax(4px, 1fr));
            gap: 1px;
        }

        .dot {
            height: 5px;
            width: 5px;
            border: 1px solid #bcc3ce;
        }
    }
}

#members-list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
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
