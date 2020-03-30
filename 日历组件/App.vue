<template>
  <div>
    <calendar />
  </div>
</template>

<script>
// 引入组件 calendar
import calendar from "./Calendar";

export default {
  data() {
    return {
      msg: "hello world"
    };
  },
  components: {
    // 也可以直接缩写成 calendar
    calendar: calendar
  }
};
</script>

<style>

</style>


