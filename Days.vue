<template>
  <div class="calendar-list">
    <div
      v-for="(item, index) in days"
      :key="'calendar' + index"
      :style="{
        height: lines === 5 ? '20%' : '16.666666%',
        borderBottom:
          (lines === 6 && index > 34) || (lines === 5 && index > 27)
            ? 'none'
            : ''
      }"
      :class="[
        'calendar-item',
        activeDay === item.dateStr ? 'active' : '',
        item.disabled ? 'disabled' : ''
      ]"
      @click="chooseDay(item)"
    >
      {{ item.date }}
    </div>
  </div>
</template>
<script>
import moment from 'moment';
export default {
  props: {
    hasGpsMapping: {
      type: Object,
      default: () => {
        return {};
      }
    },
    yearMonthStr: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      dayList: [],
      lines: 5,
      todayStr: '',
      activeDay: ''
    };
  },
  computed: {
    days() {
      const days = this.dayList.map(day => {
        if (day.dateStr === this.todayStr) {
          this.chooseDay(day);
        }
        return day;
      });
      return days;
    }
  },
  watch: {
    yearMonthStr: {
      immediate: true,
      handler(val) {
        if (val) {
          this.setDay(val);
        }
      }
    }
  },
  created() {
    this.todayStr = this.activeDay = moment().format('YYYY-MM-DD');
    this.$emit('day-active', {
      dateStr: this.todayStr
    });
  },
  methods: {
    chooseDay(item) {
      if (!item.disabled) {
        this.activeDay = item.dateStr;
        this.$emit('day-active', item);
      }
    },
    setDay(yearMonth) {
      const startMonth = moment(yearMonth).startOf('month');
      const endMonth = moment(yearMonth).endOf('month');
      const endDate = endMonth.date();
      const startMonthweek = startMonth.day();
      const endMonthweek = endMonth.day();
      const startIndex = -startMonthweek;
      const endIndex = endDate + 6 - endMonthweek;
      const list = [];
      // 判断显示多少行
      this.lines = endIndex - startIndex > 35 ? 6 : 5;
      for (let i = startIndex; i < endIndex; i++) {
        const datetime = moment(yearMonth).startOf('month').subtract(-i, 'day');
        const dateStr = datetime.format('YYYY-MM-DD');
        list.push({
          date: datetime.date(),
          dateStr,
          disabled: i < 0 || i >= endDate
        });
      }
      this.dayList = list;
    }
  }
};
</script>
<style lang="less" scoped>
.calendar-list {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  height: calc(~'100% - 68px');
  .calendar-item {
    position: relative;
    width: 14.285714%;
    height: 20%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    &.disabled {
      cursor: not-allowed;
      color: #bbb;
    }
    &.active {
      background: #2080f7;
      color: #fff;
    }
  }
}
</style>
