<template>
    <section id="balance" class="ll container">
    <article class="section">

            <header>
                <ul class="tab">
                    <li :class="`tab-item ${isOverall ? '' : 'active'}`">
                        <a @click.prevent="isOverall=false" href="#" class="">By Party</a>
                    </li>
                    <li :class="`tab-item ${isOverall ? 'active' : ''}`">
                        <a @click.prevent="isOverall=true" href="#">Overall</a>
                    </li>
                </ul>
            </header>

            <section class="flex" v-if="isOverall">
                <section>
                    <p class="ll h5">Landlords ({{ landlordCount }})</p>
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

            <section class="flex" v-if="!isOverall">
                <template v-for="(party, i) in parties" v-bind:key="i">
                    <section :style="`flex-basis: ${(party.members/memberCount)*100}%`">
                        <p class="h5">
                            <abbr :title="party.name">{{ party.abbr || 'None' }}</abbr>
                        </p>
                        <div class="grid">
                            <div class="dot ll" v-for="i in party.landlords" v-bind:key="i" :style="`background-color: #${party.colour}`"></div>
                            <div class="dot" v-for="i in (party.members-party.landlords)" v-bind:key="i" :style="`border-color: #${party.colour}`"></div>
                        </div>
                    </section>
                </template>
            </section>

            <ul class="footer">
                <li>
                    <div class="dot ll"></div> Landlord
                </li>
                <li>
                    <div class="dot"></div> Not a landlord
                </li>
            </ul>
    </article>
</section>
</template>

<script>
export default {
    name: 'BalanceBox',
    props: [
        'house'
    ],
    data() {
        return {
            isOverall: false
        }
    },
    computed: {
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
}
</script>


<style lang="scss" scoped>
#balance {
    margin: 0 0 .5rem;
    padding: 0 .33rem 1rem;

    .ll {
        color: darken(salmon, 11%);
    }

    header {
        margin: 0 0 1rem;
    }

    .flex {
        display: flex;
        align-items: flex-start;

        section {
            flex: 1 1 auto;
        }
    }

    .h5 {
        margin: 0 0 .33rem;
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

    ul.footer {
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
</style>