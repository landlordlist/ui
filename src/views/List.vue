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
<BalanceBox :house="house"></BalanceBox>
<main class="home ll container">
    <article class="section">
        <section id="members-list" :class="(isLoading ? 'loading loading-lg' : '')">
            <template v-for="(member, key) in members" v-bind:key="key">
                <PersonBox :member="member"></PersonBox>
            </template>
        </section>
    </article>
</main>
</template>

<script>
import PersonBox from '@/components/PersonBox.vue'
import BalanceBox from '@/components/BalanceBox.vue'

export default {
    name: 'List',
    components: {
        PersonBox,
        BalanceBox
    },
    data() {
        return {
            house: {},
            members: [],
            isLoading: false
        }
    },
    methods: {
        membersOffset() {
            // SQL OFFSET is 0 indexed, .length is 1 indexed
            return this.members.length
        },
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
            let r = await fetch(`https://landlordlist.uk/api/people?house=${this.$route.params.name}&offset=${this.membersOffset()}`)
            let data = await r.json()
            this.members = this.members.concat(data)
            this.isLoading = false
        },
        scroll() {
            window.onscroll = () => {
                let trigger = document.documentElement.scrollTop + window.innerHeight > (document.documentElement.offsetHeight - 100);

                if (trigger) {
                    this.getMoreMembers()
                }
            }
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
    },
    mounted() {
        this.scroll()
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
