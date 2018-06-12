<template>
  <div>
    <ul>
      <div v-for="date in Object.keys(events)" :key="date">
        <h3>{{ moment(date).format('MMM Do') }}</h3>
        <li v-for="item in events[date]" :key="item.id">
          <div class="event">
            <div class="event-name">
              <b>{{ item.name  }}</b>
            </div>
            <div class="event-group">
              {{ item.group.name }}
            </div>
            <div class="event-time">
              {{ time(item) }}
            </div>
          </div>
        </li>
      </div>
    </ul>
  </div>
</template>

<script>
import * as moment from 'moment';
import meetups from '../assets/meetups.json';

/* eslint-disable no-param-reassign */
const groupBy = (xs, key) => (
  xs.reduce((rv, x) => {
    (rv[x[key]] = rv[x[key]] || []).push(x);
    return rv;
  }, {})
);

const friendlyTime = str => (moment(str).format('hh:mm A'));

export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
    };
  },
  computed: {
    events() {
      return groupBy(meetups.events, 'local_date');
    },
  },

  methods: {
    moment,
    time: (item) => {
      const start = friendlyTime(item.time);
      if (item.duration) {
        const end = friendlyTime(item.time + item.duration);
        return `${start} - ${end}`;
      } else {
        return `${start} - ?`;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2, h3 {
  font-weight: normal;
}
ul {
  padding: 0;
  list-style-type: none;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}

.event {
  margin: 10px 0;
}
</style>
