<template>
  <div class="text-center">
    <h4 v-if="selectedDate">Fecha seleccionada: {{ formatDate(selectedDate) }}</h4>

    <div class="datepicker">
      <input
        type="text"
        class="form-control"
        v-model="selectedMonth"
        @focus="toggleDatepicker"
        :readonly="true"
        ref="datepickerInput"
      />

      <div class="datepicker-popup" v-if="showDatepicker">
        <div class="datepicker-header">
          <button class="btn btn-link" @click="goToPreviousYear" :disabled="isPreviousYearDisabled">&lt;</button>
          <span>{{ formatDate(selectedMonth) }}</span>
          <button class="btn btn-link" @click="goToNextYear">&gt;</button>
        </div>

        <div class="datepicker-body">
          <!-- Renderiza los meses y años aquí -->
          <div
            v-for="month in months"
            :key="month"
            @click="selectMonth(month)"
            :class="{ 'disabled': isMonthDisabled(month) }"
            class="month-button"
          >
            {{ formatDate(month) }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs';

export default {
  data() {
    return {
      selectedMonth: dayjs().startOf('month'), // Asegurarse de que selectedMonth sea un objeto dayjs
      showDatepicker: false,
      selectedDate: null,
    };
  },
  computed: {
    months() {
      const months = [];
      const startMonth = this.selectedMonth.clone().startOf('year');

      for (let i = 0; i < 12; i++) {
        months.push(startMonth.add(i, 'month'));
      }

      return months;
    },
  },
  methods: {
    toggleDatepicker() {
      this.showDatepicker = !this.showDatepicker;
    },
    selectMonth(month) {
      this.selectedMonth = dayjs(month).startOf('month'); // Asegurarse de que selectedMonth sea un objeto dayjs
      this.showDatepicker = false;
      this.selectedDate = this.selectedMonth;
      console.log({ selectedMonth: this.selectedMonth.month() + 1, selectedYear: this.selectedMonth.year() });
    },
    goToPreviousYear() {
      if (!this.isPreviousYearDisabled) {
        this.selectedMonth = this.selectedMonth.subtract(1, 'year');
      }
    },
    goToNextYear() {
      this.selectedMonth = this.selectedMonth.add(1, 'year');
    },
    isPreviousYearDisabled() {
      const currentMonth = dayjs();
      return this.selectedMonth.isBefore(currentMonth, 'year');
    },
    isMonthDisabled(month) {
      const currentMonth = dayjs();
      return dayjs(month).isBefore(currentMonth, 'month'); // Asegurarse de que month sea un objeto dayjs
    },
    formatDate(date) {
      return dayjs(date).format('MMMM YYYY');
    },
  },
  mounted() {
    // Enfocar automáticamente el input al cargar el componente
    this.$refs.datepickerInput.focus();
  },
};
</script>

<style scoped>
.datepicker {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.datepicker-popup {
  position: absolute;
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 10px;
  z-index: 1000;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.datepicker-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.datepicker-body {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.month-button {
  cursor: pointer;
  padding: 10px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.month-button:hover {
  background-color: #f0f0f0;
}

.month-button.disabled {
  cursor: not-allowed;
  background-color: #eee;
}

.btn-link {
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  font-size: inherit;
  color: #007bff;
  text-decoration: underline;
}
</style>
