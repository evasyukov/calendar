<template>
  <div>
    <calendar
      :language="'ru'"
      @dateSelected="handleDateSelected"
    ></calendar>
  </div>
</template>

<script>
import Calendar from "./components/Calendar.vue"

export default {
  components: {
    Calendar,
  },
  methods: {
    handleDateSelected(date) {

    },
  },
}
</script>

<style scoped></style>
