<template>
  <div class="calendar">
    <div class="date-header">
      <div class="prev-month" @click="prev"></div>
      <div class="show-date">{{ year }}年{{ month }}月{{ day }}日</div>
      <div class="next-month" @click="next"></div>
    </div>
    <div class="adte-content">
      <div class="week-header">
        <div v-for="item in ['日','一','二','三','四','五','六']" :key="item">{{ item }}</div>
      </div>
      <ul class="week-day">
        <li class="every-day" v-for="item in 42" :key="item">
          <!-- 上月 -->
          <div v-if="(item - getWeek) <= 0" class="other-day">{{ item - getWeek + getPrevMonth }}</div>

          <!-- 当前月 -->
          <div
            v-else-if="(item - getWeek) > 0 && (item - getWeek) <= monthDay"
            @click="active(item)"
            :class="{
              curDay: `${year}-${month}-${item - getWeek}` == curDay,
              active: `${year}-${month}-${item - getWeek}` == `${year}-${month}-${clickDay}`
            }"
          >{{ item - getWeek}}</div>

          <!-- 下月 -->
          <div v-else class="other-day">{{ item - monthDay }}</div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      year: null,
      month: null,
      day: null,
      curDay: null,
      clickDay: null
    };
  },
  // 加载完后渲染时间
  mounted() {
    this.getInitTime();
  },
  methods: {
    // 初始化获取当前时间
    getInitTime() {
      let date = new Date();
      this.year = date.getFullYear();
      // getMonth 0 开始 所以需要 + 1
      this.month = date.getMonth() + 1;
      this.day = date.getDate();
      // 今天日期
      this.curDay = `${this.year}-${this.month}-${this.day}`;
    },

    // 点击日期
    active(targetday) {
      console.log(targetday);
      this.clickDay = targetday;
    },

    // 切换月份
    prev() {
      if (this.month == 1) {
        this.month = 12;
        this.year--;
      } else {
        this.month--;
      }
    },
    next() {
      if (this.month == 12) {
        this.month = 1;
        this.year++;
      } else {
        this.month++;
      }
    }
  },
  computed: {
    // 获得当前月的最后一天（总天数）
    monthDay() {
      // 在构造日期对象时，月份是从0开始的。而第三个数0天数，要求最小是1号，比1号小的话，就成了获取月份的最后一天
      return new Date(this.year, this.month, 0).getDate();
    },
    getWeek() {
      // 获得当前月份1号是星期几
      return new Date(this.year, this.month - 1, 1).getDay();
    },
    getPrevMonth() {
      // 获得上个月总天数
      return new Date(this.year, this.month - 1, 0).getDate();
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
}

.calendar {
  margin: 50px auto 0;
  width: 500px;
}

.date-header {
  line-height: 30px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  font-weight: bold;
}

.prev-month,
.show-date,
.next-month {
  color: #007fff;
}

.prev-month,
.next-month {
  border: 15px solid transparent;
  width: 0;
  height: 0;
  cursor: pointer;
}

.prev-month {
  border-right-color: #007fff;
}

.next-month {
  border-left-color: #007fff;
}

.week-header {
  width: 100%;
  display: flex;
  justify-content: space-around;
  background: #007fff;
  color: #fff;
  font-weight: 600;
  line-height: 30px;
}

.every-day {
  width: 14.28%;
  line-height: 50px;
  text-align: center;
  display: inline-block;
  cursor: pointer;
}

.other-day {
  color: #ccc;
}

.curDay {
  background: #007fff;
  color: #fff;
  font-weight: 600;
}

.active {
  border: 2px solid #007fff;
  line-height: 46px;
}
</style>



