<template>
    <article class="card">
        <div class="card-header">
            <img class="avatar" :src="member.avatar_url">
            <div>
                <div class="chip" 
                :style="`background-color: #${member.party.colour}; color: #${member.party.text_colour}`">
                    {{ member.party.name }}
                </div>

                <h5 class="card-title h5">{{member.name}}</h5>
                <span class="card-subtitle text-gray">{{ member.from}}</span>
            </div>
        </div>
        <div class="card-footer">
            <p class="landlord text-primary" v-if="member.is_landlord">Landlord</p>
            <p class="not text-gray" v-if="!member.is_landlord">Not A Landlord</p>
            <button v-if="member.is_landlord" class="btn btn-link">Details</button>
        </div>
    </article>
</template>

<script>
export default {
    name: 'PersonBox',
    props: [
        'member'
    ]
}
</script>


<style lang="scss" scoped>
img.avatar {
    object-fit: cover;
    height: 100px;
    width: 100px;
    margin: 0 .66rem .33rem 0;
    border-radius: 3px;
    border: 1px solid whitesmoke;
    background: transparent;
}

.card-header {
    display: flex;
    > div {
        width: 100%;
    }
}

.landlord {
    font-size: 30pt;
    text-transform: uppercase;
    font-weight: 900;
    border: 3px solid salmon;
    padding: .1em .2em;
    line-height: 1em;
    margin: 0;
    width: min-content;
}

.not {
    font-size: 22pt;
    font-weight: 800;
    line-height: 1em;
    margin: .5rem 0 0;
}

.chip {
    float: right;
}

.card-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>