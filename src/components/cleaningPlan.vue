<template>
  <h1>Cleaning Plan</h1>
  <table>
    <thead>
    <tr>
      <th>Cleaned until</th>
      <th v-for="area in areas">{{ area }}</th>
    </tr>
    <tr v-for="(date,i) in dates" :key="date">
      <td>{{ date }} </td>
      <textAndButton v-for="(_,offset) in cleaners">{{ cleaners[(i + offset + weekCorrection + 2) % 3] }}</textAndButton>
    </tr>
    </thead>
  </table>

</template>

<script>
import TextAndButton from "@/components/textAndButton.vue";

export default {
  name: "cleaningplan",
  components: {TextAndButton},
  data() {
    return {
      areas: ['Wohnzimmer + Küche', 'Toilette', 'Bad + Gang'],
      cleaners: ['Dominik','Gregor', 'Marvin'],
      numOfDates: 6,
      weekCorrection: 0,
    };
  },

  computed: {
    dates() {
      const result = [];
      const currentDate = new Date()

      this.weekCorrection = this.getWeeksSinceStartDate('2023-01-05', currentDate) % 3 +1;
      // Find the previous Wednesday
      const dist = (new Date(currentDate).getDay() - 3 + 7) % 7;
      const previousWednesday = new Date(currentDate.getFullYear(), currentDate.getMonth(), +currentDate.getDate() - (dist || 7));
      // Add the previous Wednesday to the result
      result.push(previousWednesday.toDateString());

      // Generate the next Wednesdays
      let count = 0;
      while (count < this.numOfDates) {
        const nextWednesday = previousWednesday;
        nextWednesday.setDate(previousWednesday.getDate() + 7);

        result.push(nextWednesday.toDateString());
        previousWednesday.setDate(nextWednesday.getDate());
        count++;
      }

      return result;
    }
  },

  methods: {
    getWeeksSinceStartDate(startDate, today) {
      const start = new Date(startDate); // Convert the start date string to a Date object

      // Calculate the time difference in milliseconds
      const timeDiff = today.getTime() - start.getTime();

      // Calculate the number of weeks
      const weeks = Math.floor(timeDiff / (1000 * 60 * 60 * 24 * 7));
      console.log(weeks)

      return weeks;
    }
  }
};

</script>


<style>

</style>