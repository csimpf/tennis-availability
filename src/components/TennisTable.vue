<script>
const API_URL =
  "https://play.tennis.com.au/v0/BookACourtVenue/WestEppingParkTennisCourts/GetVenueSessions?startDate=2022-08-18&endDate=2022-08-21";
const DAY_OF_WEEK = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
export default {
  name: "tennisTable",
  data() {
    return {
      msg: null,
      debug: null,
    };
  },
  created() {
    fetch(API_URL)
      .then((rsp) => rsp.json().then((json) => (this.msg = json)))
      .catch((e) => (this.debug = e));
  },

  methods: {
    mapCategory(num) {
      switch (num) {
        case 1e3:
          return "BOOKING";
        case 2e3:
          return "COACHING";
        case 7e3:
          return "MAINTENANCE";
        case 0:
          return "FREE";
        default:
          return num;
      }
    },
    displayDate(date) {
      const temp = new Date(date);
      return DAY_OF_WEEK[temp.getDay()] + " " + temp.toLocaleDateString();
    },
  },

  computed: {
    computedMessage() {
      return this.msg;
    },
    resourcesArray() {
      return this.msg?.Resources;
    },
  },
};
</script>

<template>
  <div class="main">
    <div class="courts" v-for="resource in resourcesArray" :key="resource.Name">
      <h1>{{ resource.Name }}</h1>
      <table v-for="day in resource.Days" :key="day.Date">
        <tr>
          <th>{{ displayDate(day.Date) }}</th>
        </tr>
        <tr v-for="sess in day.Sessions" :key="sess.ID + '' + sess.StartTime">
          <td
            :class="[
              mapCategory(sess.Category),
              sess.Recurrence && 'recurrence',
            ]"
            :style="{ height: ((sess.Interval * 2) / 60) * 13.5 - 1 + 'px' }"
          >
            {{
              Math.floor(sess.StartTime / 60) * 100 +
              ((sess.StartTime / 60) % 1) * 60
            }}
            | {{ mapCategory(sess.Category) }} | {{ sess.Interval / 60 }} h
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
.main {
  overflow: auto;
  white-space: nowrap;
  animation-duration: 2sec;
}

table {
  font-size: 0.5em;

  border-collapse: collapse;
  border-radius: 4px;
  background-color: lightgrey;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  display: inline-block;
  vertical-align: top;
}

table td {
  border: 1px solid darkgrey;
  padding: 0;
  background-color: lightgrey;
}

td:hover {
  filter: brightness(95%);
}

.recurrence {
  background-color: rgb(208, 197, 194);
}
.FREE {
  background-color: lightgreen;
}
</style>
