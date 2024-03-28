<template>
  <div class="calendar">
    <div class="header">
      <button @click="prevMonth">&lt;</button>
      <div class="month-year">{{ currentMonthName }} {{ currentYear }}</div>
      <button @click="nextMonth">&gt;</button>
      <LanguageSwitcher
        :language="language"
        @language-change="changeLanguage"
      ></LanguageSwitcher>
    </div>
    <div class="days-of-week">
      <div v-for="day in daysOfWeek" :key="day">{{ day }}</div>
    </div>
    <div class="days">
      <div
        v-for="day in days"
        :key="day.date"
        @click="selectDate(day.date)"
        :class="{ empty: !day.day, selected: day.date === selectedDate }"
      >
        {{ day.day }}
      </div>
    </div>
  </div>
</template>

<script>
import { localization } from "../localization"
import LanguageSwitcher from "./LanguageSwitcher.vue"

export default {
  name: "Calendar",
  components: {
    LanguageSwitcher,
  },
  props: {
    initialDate: {
      type: String,
      default: "",
    },
    language: {
      type: String,
      default: "ru",
    },
  },
  data() {
    return {
      currentMonth: 0,
      currentYear: 0,
      days: [],
      selectedDate: "",
    }
  },
  created() {
    this.initCalendar()
  },
  methods: {
    initCalendar() {
      const date = this.initialDate ? new Date(this.initialDate) : new Date()
      this.currentMonth = date.getMonth()
      this.currentYear = date.getFullYear()
      this.generateDays()
    },
    generateDays() {
      const firstDayOfMonth = new Date(
        this.currentYear,
        this.currentMonth,
        1
      ).getDay()
      const daysInMonth = new Date(
        this.currentYear,
        this.currentMonth + 1,
        0
      ).getDate()
      const daysArray = []

      for (let i = 1; i <= firstDayOfMonth; i++) {
        daysArray.push({ day: "", date: "" })
      }

      for (let i = 1; i <= daysInMonth; i++) {
        daysArray.push({
          day: i,
          date: `${this.currentYear}-${this.currentMonth + 1}-${i}`,
        })
      }

      this.days = daysArray
    },
    prevMonth() {
      if (this.currentMonth === 0) {
        this.currentMonth = 11
        this.currentYear--
      } else {
        this.currentMonth--
      }
      this.generateDays()
    },
    nextMonth() {
      if (this.currentMonth === 11) {
        this.currentMonth = 0
        this.currentYear++
      } else {
        this.currentMonth++
      }
      this.generateDays()
    },
    selectDate(date) {
      this.selectedDate = date
      this.$emit("dateSelected", this.selectedDate)
    },
  },
  computed: {
    currentMonthName() {
      return localization[this.language].months[this.currentMonth]
    },
    daysOfWeek() {
      return localization[this.language].daysOfWeek
    },
  },
}
</script>

<style scoped>
.calendar {
  width: 600px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #494747;
  border-bottom: 1px solid #ddd;
}

.month-year {
  font-size: 18px;
  font-weight: bold;
}

.days-of-week,
.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 1px;
}

.days-of-week {
  background-color: #494747;
  padding: 5px 0;
}

.days div {
  padding: 10px;
  text-align: center;
  border: 1px solid #ddd;
}

.days div.empty {
  background-color: #ffffff44;
}

.days div.selected {
  background-color: #cecdcda1;
  color: white;
}
</style>./LanguageSwitcher.vue
