<template>
<header id="site-header" class="container">
    <article class="section">
        <router-link to="/"><img src="./../assets/logo.svg"></router-link>
    </article>
</header>

<section class="ll container">
    <article class="section banner">
        <router-link to="/">
            <i class="icon icon-back"></i>
            Back
        </router-link>
    </article>
</section>

<section class="ll container">
    <header class="section banner">
        <h1>House of {{ house.name }} </h1>
    </header>
</section>

<BalanceBox class="strip" :house="house"></BalanceBox>

<section class="ll container">
    <article class="section banner toolbar">
         <input v-model="search" class="form-input" type="search" placeholder="Search" @input="doSearch()">
         <label class="form-switch">
            <input v-model="landlordsOnly" type="checkbox" @change="getMembers()">
            <i class="form-icon"></i> Landlords
        </label>
    </article>
</section>

<main class="home ll container">
    <article class="section">
        <section id="members-list" :class="(isLoading ? 'loading loading-lg' : '')">
            <template v-for="(member, key) in members" v-bind:key="key">
                <PersonBox :member="member"></PersonBox>
            </template>
            <template v-if="!members.length">
                <div class="empty">
                    <div class="empty-icon">
                        <i class="icon icon-people"></i>
                    </div>
                    <p class="empty-title h5">There were no results</p>
                    <p class="empty-subtitle">Remove your search term from the search box to reload all people.</p>
                </div>
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
            isLoading: false,
            search: '',
            landlordsOnly: false
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

            let url = `https://landlordlist.uk/api/people/?house=${this.$route.params.name}`

            if (this.landlordsOnly) {
                url += '&landlord=1'
            }

            if (this.search != '') {
                url += `&search=${this.search}`
            }

            let r = await fetch(url)
            let data = await r.json()
            this.members = data
            this.isLoading = false
        },
        async getMoreMembers() {
            if (this.isLoading) {
                return
            }
            this.isLoading = true

            let url = `https://landlordlist.uk/api/people/?house=${this.$route.params.name}&offset=${this.membersOffset()}`

            if (this.landlordsOnly) {
                url += '&landlord=1'
            }

            if (this.search != '') {
                url += `&search=${this.search}`
            }

            let r = await fetch(url)
            let data = await r.json()
            this.members = this.members.concat(data)
            
            this.isLoading = false
        },
        scroll() {
            window.scrollTo(0,0)

            window.onscroll = () => {
                let trigger = document.documentElement.scrollTop + window.innerHeight > (document.documentElement.offsetHeight - 100);

                if (trigger) {
                    this.getMoreMembers()
                }
            }
        },
        doSearch() {
            if (this.isLoading) {
                return
            }

            if (this.search == '') {
                this.getMembers()
            }
            
            if (this.search.length >= 3) {
                this.getMembers()
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
        width: clamp(100px, 200px, 45vw);
        min-height: 50px;
        margin: 0;
    }

    >.section {
        display: flex;
        justify-content: center;
        align-items: center;
    }

}

input {
    margin-right: .66rem;
}

.toolbar {
    display: flex;
    justify-content: space-between;
    padding: 1rem .33rem 0;
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
