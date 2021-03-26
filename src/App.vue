<template>
  <div>
    <datePicker
      :options="calendarArr"
      class="calendar"
      @handleClickDay="handleClickDay"
      @handlePrevMonth="handlePrevMonth"
      @handleNextMonth="handleNextMonth"
    />
    <div style="display: flex; justify-content: flex-end; margin-top: 15px">
      <div style="margin-right: 30px">
        <el-button type="primary" @click="changeType(1)">单独设置</el-button>
      </div>
      <div style="margin-right: 30px">
        <el-button type="primary" @click="changeType(2)"
          >按日期范围设置</el-button
        >
      </div>
      <div style="margin-right: 30px">
        <el-button type="primary" @click="changeType(3)">自由设置</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import datePicker from './components/date-picker'
export default {
  name: 'date-picker',
  data() {
    return {
      calendarArr: {
        type: 'combination',
        headStyle: {
          todayBtn: 'right',
          combination: 'center',
          checkBtn: 'right',
        },
        viewStyle: {
          day: 'right'
        },
        calendarData: [],
        pricetype: 1,//价格设置类型
      },
    }
  },
  components: {
    datePicker
  },
  methods: {
    handleClickDay() {

    },
    handlePrevMonth() {

    },
    handleNextMonth() {

    },
    changeType(type) {
      this.calendarArr.pricetype = type
      console.log(this.calendarArr.pricetype)
    }
  }
}
</script>

<style>
</style>
