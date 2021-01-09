<template>
  <div v-if="loaded">
    <section class="text-center my-2 text-3xl font-light">
      <h5 v-if="!expired">Time</h5>
      <h5 v-else>Time Out</h5>
    </section>
    <section class="font-light mt-2 text-6xl flex justify-center">
      <div class="mr-2 relative">
        {{ displayDays }}
        <div class="label absolute text-sm bottom-0">days</div>
      </div>
      <span class="leading-snug">:</span>
      <div class="mx-2 relative">
        {{ displayHours }}
        <div class="label absolute text-sm bottom-0">hours</div>
      </div>
      <div class="leading-snug">:</div>
      <div class="mx-2 relative">
        {{ displayMinutes }}
        <div class="label absolute text-sm bottom-0">minutes</div>
      </div>
      <div class="leading-snug">:</div>
      <div class="seconds ml-2 relative">
        {{ displaySeconds }}
        <div class="label absolute text-sm bottom-0">seconds</div>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  props: ["year", "month", "date", "hour", "minute", "second", "millisecond"],
  data() {
    return {
      displayDays: 0,
      displayHours: 0,
      displayMinutes: 0,
      displaySeconds: 0,
      loaded: false,
      expired: false,
    };
  },
  computed: {
    _seconds: () => 1000,
    _minutes() {
      return this._seconds * 60;
    },
    _hours() {
      return this._minutes * 60;
    },
    _days() {
      return this._hours * 24;
    },
    _end() {
      return new Date(
        this.year,
        this.month,
        this.date,
        this.hour,
        this.minute,
        this.second,
        this.millisecond
      );
    },
  },
  mounted() {
    this.show();
  },
  methods: {
    formatNum: (num) => (num < 10 ? "0" + num : num),
    show() {
      const timer = setInterval(() => {
        const now = new Date();
        /* const end = new Date(2021, 1, 8, 10, 10, 10, 10); */
        const distance = this._end.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          this.expired = true;
          return;
        }

        const days = Math.floor(distance / this._days);
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);

        this.displayMinutes = this.formatNum(minutes);
        this.displaySeconds = this.formatNum(seconds);
        this.displayHours = this.formatNum(hours);
        this.displayDays = this.formatNum(days);
        this.loaded = true;
      }, 1000);
    },
  },
};
</script>
<style scoped>
.seconds {
  max-width: 60px;
}
</style>