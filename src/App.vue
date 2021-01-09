<template>
  <div id="app">
    <h1 class="text-center font-light text-4xl">Timer With Vue Js</h1>
    <Count
      :year="2021"
      :month="2"
      :date="1"
      :hour="0"
      :minute="0"
      :second="0"
      :millisecond="0"
    />
  </div>
</template>

<script>
import Count from "./components/Countdown";

export default {
  name: "App",
  components: {
    Count,
  },
};
</script>