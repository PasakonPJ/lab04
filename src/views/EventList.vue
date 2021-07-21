<template>
  <span>
    <select id="selectedpage" v-model.number="selectedpage">
      <option>1</option>
      <option>2</option>
      <option>3</option>
      <option>4</option>
    </select>
    <h1>Events For Good</h1>
  </span>

  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <div class="pagination">
      <router-link
        id="page-prev"
        :to="{ name: 'EventList', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
      >
        Prev page
      </router-link>

      <router-link
        id="page-next"
        :to="{ name: 'EventList', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPage"
      >
        Next page
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { watchEffect } from '@vue/runtime-core'
export default {
  name: 'EventList',
  props: {
    page: {
      type: Number,
      required: true
    },
    perpage: {
      type: Number,
      required: true
    }
  },
  components: {
    EventCard
  },
  data() {
    return {
      events: null,
      totalEvents: 0, //<-- Added this to store totalEvents
      selectedpage: this.perpage
    }
  },
  created() {
    watchEffect(() => {
      EventService.getEvents(this.selectedpage, this.page)
        .then((response) => {
          console.log(response)
          this.events = response.data
          this.totalEvents = response.headers['x-total-count'] //<-- Store it
        })
        .catch((error) => {
          console.log(error)
        })
    })
  },
  computed: {
    hasNextPage() {
      //First, calculate total pages
      let totalPages = Math.ceil(this.totalEvents / this.selectedpage) // 2 is events per page
      //then check to see if the current page is less than the total pages.
      return this.page < totalPages
    }
  }
}
</script>
<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: rgb(20, 85, 55);
}
#page-prev {
  text-align: left;
}
#page-next {
  text-align: right;
}
#selectedpage {
  flex: 1;
  text-align: left;
  flex-direction: column;
}
</style>
