<template>
    <div v-if="event">
        <h1>{{ event.title }}</h1>
        <div id="nav">
            <router-link :to="{ name: 'EventDetails', params: { id } }">
                Details
            </router-link> |
            <router-link :to="{ name: 'EventDetails', params: { id } }">
                Register
            </router-link> |
            <router-link :to="{ name: 'EventDetails', params: { id } }">
                Edit
            </router-link>
        </div>
        <p>Edit the event here</p>
    </div>
</template>

<script>
export default {
    props: ['id'],
    data() {
        return{
            event: null
        }
    },
    created() {
        EventService.getEvent(this.id)
        .then((response) => {
            this.event = response.data
        })
        .catch((error) => {
            console.log(error)
        })
    }
}
</script>