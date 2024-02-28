<template>
  <div class="buttons">
    <i class="h-icon-angles_left_sm" @click="changeYear('left')"></i>
    <i class="h-icon-angle_left_sm" @click="changeMonth('left')"></i>
    <div style="width: 48px">{{ year }}</div>
    <span>年</span>
    <div style="width: 28px">{{ month }}</div>
    <span>月</span>
    <i class="h-icon-angle_right_sm" @click="changeMonth('right')"></i>
    <i class="h-icon-angles_right_sm" @click="changeYear('right')"></i>
  </div>
</template>
<script>
import moment from 'moment';
export default {
  data() {
    return {
      yearMonthStr: '',
      year: 2022,
      month: 1,
      currentYear: 23,
      currentMonth: 1
    };
  },
  created() {
    const { startTime } = this.$route.query;
    // 第三方嵌入
    if (!(this.$route.name === 'CarTrack' && startTime)) {
      const current = moment();
      this.currentYear = current.year();
      this.currentMonth = current.month() + 1;
      this.setYearMonth(current);
    }
  },
  methods: {
    setDefaultYearMonth(dateStr) {
      this.setYearMonth(moment(dateStr));
    },
    setYearMonth(datetime) {
      this.yearMonthStr = datetime.format('YYYY-MM');
      this.year = datetime.year();
      this.month = datetime.month() + 1;
      this.$emit('year-month-change', this.yearMonthStr);
    },
    changeYear(dir) {
      const datetime = moment(this.yearMonthStr);
      if (dir === 'left') {
        this.setYearMonth(datetime.subtract(1, 'year'));
      } else if (dir === 'right') {
        this.setYearMonth(datetime.subtract(-1, 'year'));
      }
    },
    changeMonth(dir) {
      const datetime = moment(this.yearMonthStr);
      if (dir === 'left') {
        this.setYearMonth(datetime.subtract(1, 'month'));
      } else if (dir === 'right') {
        this.setYearMonth(datetime.subtract(-1, 'month'));
      }
    }
  }
};
</script>
<style lang="less" scoped>
.buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 32px;
  font-size: 14px;
  line-height: 24px;
  div {
    text-align: right;
    padding: 0 4px 0 0;
  }
  i {
    font-size: 24px;
    cursor: pointer;
    margin: 0 4px;
    &.disabled {
      cursor: not-allowed;
      opacity: 0.5;
    }
    &:hover {
      background: rgba(0, 0, 0, 0.24);
    }
  }
}
</style>
