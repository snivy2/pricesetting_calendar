<template>
  <div class="content">
    <div class="cc-calendar">
      <calendarHeader
        :headOptions="headOptions"
        @handlePrevMonth="handlePrevMonth"
        @handleNextMonth="handleNextMonth"
        @handleToday="handleToday"
      />
      <ul class="calendar-week clear">
        <li
          v-for="(item, index) in calendarTitleArr"
          :key="index"
          class="week-item"
        >
          {{ item }}
        </li>
      </ul>
      <ul class="calendar-view clear">
        <li
          v-for="(item, index) in calendarList"
          :key="index"
          class="date-view"
          :class="[
            { 'month-class': !isCurrentMonth(item.date) },
            { todayBg: isCurrentDay(item.date) },
            { handleDay: item.clickDay },
          ]"
          @click="handleClickDay(item, index)"
        >
          <span
            class="date-day"
            :style="dayStyle"
            :class="[{ 'opacity-class': !isCurrentMonth(item.date) }]"
          >
            {{ item.day }}
          </span>
          <span class="calendar-num">
            {{ item.calendarNum }}
          </span>
          <div class="productNum">余{{ item.num }}</div>
          <div class="productCountPrice">￥{{ item.countPrice }}</div>
          <div class="productPrice">￥{{ item.price }}</div>
        </li>
      </ul>
    </div>
    <div class="formDiv">
      <div class="formCaseA" v-if="pricetype == 1">
        <div>价格设置</div>
        <div>2021年3月14日</div>
        <div class="formContent">
          <div>单位：元</div>
          <div>成人</div>
          <div>儿童</div>
          <div>老人</div>
        </div>
        <div class="formContent">
          <div>单位：元</div>
          <div>成人</div>
          <div>儿童</div>
          <div>老人</div>
        </div>
        <div class="formContent">
          <div>门市价</div>
          <div>
            <el-input
              v-model="priceForm.priceA"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.priceB"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.priceC"
              placeholder="请输入内容"
            ></el-input>
          </div>
        </div>
        <div class="formContent">
          <div>结算价</div>
          <div>
            <el-input
              v-model="priceForm.countPriceA"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.countPriceB"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.countPriceC"
              placeholder="请输入内容"
            ></el-input>
          </div>
        </div>
        <div class="formFooter">
          <div>库存</div>
          <div>
            计划人数：<el-input-number
              v-model="priceForm.peopleNum"
              controls-position="right"
              @change="handleChange"
              :min="0"
            ></el-input-number>
          </div>
          <div>已定：{{ priceForm.orderNum }}</div>
          <div>余：{{ priceForm.peopleNum - priceForm.orderNum }}</div>
        </div>
        <div class="btnContent">
          <el-button type="primary" size="medium" @click="setOnePrice"
            >应用</el-button
          >
        </div>
      </div>
      <div class="formCaseB" v-if="pricetype == 2">
        <div>价格设置</div>
        <div style="text-align: center; margin-top: 15px">
          <el-date-picker
            v-model="formtime"
            type="daterange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            :picker-options="pickerBeginDateBefore"
          >
          </el-date-picker>
        </div>
        <div class="formContent">
          <div>单位：元</div>
          <div>成人</div>
          <div>儿童</div>
          <div>老人</div>
        </div>
        <div class="formContent">
          <div>门市价</div>
          <div>
            <el-input
              v-model="priceForm.priceA"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.priceB"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.priceC"
              placeholder="请输入内容"
            ></el-input>
          </div>
        </div>
        <div class="formContent">
          <div>结算价</div>
          <div>
            <el-input
              v-model="priceForm.countPriceA"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.countPriceB"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.countPriceC"
              placeholder="请输入内容"
            ></el-input>
          </div>
        </div>
        <div class="formFooter">
          <div>库存</div>
          <div>
            计划人数：<el-input-number
              v-model="priceForm.peopleNum"
              controls-position="right"
              @change="handleChange"
              :min="0"
            ></el-input-number>
          </div>
          <div>最多已定：23</div>
        </div>
        <div class="btnContent">
          <el-button type="primary" size="medium" @click="setRangePrice"
            >应用</el-button
          >
          <el-button size="medium">取消</el-button>
        </div>
      </div>
      <div class="formCaseC" v-if="pricetype == 3">
        <div>价格设置</div>
        <div class="formContent">
          <div>单位：元</div>
          <div>成人</div>
          <div>儿童</div>
          <div>老人</div>
        </div>
        <div class="formContent">
          <div>门市价</div>
          <div>
            <el-input
              v-model="priceForm.priceA"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.priceB"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.priceC"
              placeholder="请输入内容"
            ></el-input>
          </div>
        </div>
        <div class="formContent">
          <div>结算价</div>
          <div>
            <el-input
              v-model="priceForm.countPriceA"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.countPriceB"
              placeholder="请输入内容"
            ></el-input>
          </div>
          <div>
            <el-input
              v-model="priceForm.countPriceC"
              placeholder="请输入内容"
            ></el-input>
          </div>
        </div>
        <div class="formFooter">
          <div>库存</div>
          <div>
            计划人数：<el-input-number
              v-model="priceForm.peopleNum"
              controls-position="right"
              @change="handleChange"
              :min="0"
            ></el-input-number>
          </div>
          <div>已选：{{ indexList.length }}天</div>
        </div>
        <div class="btnContent">
          <el-button
            @click="chooseDate"
            type="primary"
            size="medium"
            v-show="!choosn"
            >选择日历</el-button
          >
          <el-button
            type="primary"
            size="medium"
            @click="setPrice"
            v-show="choosn"
            >应用</el-button
          >
          <el-button size="medium">取消</el-button>
        </div>
        <div>
          <span style="color: red">*</span
          >点击“选择日历”按钮后在左侧日历点击想要应用价格和库存的日期。
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import '../assets/css/reset.min.css'
import calendarHeader from './canlendar-head.vue';
import * as utils from '../assets/js/utils.js';

export default {
  name: 'cc-calendar',
  componentName: 'cc-calendar',
  props: {
    options: Object
  },
  components: {
    calendarHeader
  },
  data() {
    let { year, month, day } = utils.getNewDate(new Date());
    return {
      num: 0,
      index: null,
      indexList: [],
      headOptions: {
        type: this.options.type,
        style: this.options.headStyle,
        date: '',
      },
      priceForm: {
        priceA: 0,
        priceB: 0,
        priceC: 0,
        countPriceA: 0,
        countPriceB: 0,
        countPriceC: 0,
        peopleNum: 50,
        orderNum: 10
      },
      pricetype: this.options.pricetype,
      calendarTitleArr: [
        '一',
        '二',
        '三',
        '四',
        '五',
        '六',
        '日'
      ],
      time: { year, month, day },
      calendarList: [],
      input: '',
      choosn: false,
      formtime: [],
      pickerBeginDateBefore: {
        disabledDate: (time) => {
          return (time.getTime() > new Date(`${this.time.year}-${this.time.month + 2}-01`).getTime() - 86400000) || time.getTime() < (new Date(`${this.time.year}-${this.time.month + 1}-01`).getTime())    //
        }
      }
    }
  },
  watch: {
    options: {//父组件param对象改变会触发此函数,切换组件价格设置类型
      handler(newValue, oldValue) {
        this.pricetype = newValue.pricetype
        //重置已选定的相关状态
        this.calendarList.map(x => {
          x.clickDay = false;
        });
        this.index = null
        this.indexList = []
      },
      deep: true
    }

  },
  computed: {
    dayStyle: function () {
      return {
        textAlign: this.options.viewStyle.day,
      }
    },
  },
  methods: {
    visibleCalendar() {
      let calendatArr = [];
      let { year, month, day } = utils.getNewDate(utils.getDate(this.time.year, this.time.month, 1));

      let currentFirstDay = utils.getDate(year, month, 1);

      // 获取当前月第一天星期几
      let weekDay = currentFirstDay.getDay();
      let startTime = null

      if (weekDay == 0) {
        startTime = currentFirstDay - 6 * 24 * 60 * 60 * 1000;
      } else {
        startTime = currentFirstDay - (weekDay - 1) * 24 * 60 * 60 * 1000;
      }

      let monthDayNum;
      if (weekDay == 5 || weekDay == 6 || weekDay == 0) {
        monthDayNum = 42
      } else {
        monthDayNum = 35
      };

      for (let i = 0; i < monthDayNum; i++) {
        calendatArr.push({
          date: new Date(startTime + i * 24 * 60 * 60 * 1000),
          year: year,
          month: month + 1,
          day: new Date(startTime + i * 24 * 60 * 60 * 1000).getDate(),
          clickDay: false,
          num: 10,
          price: 1800,
          countPrice: 1500
        })
      };
      this.headOptions.date = `${utils.englishMonth(month)} ${year}`;
      this.calendarList = calendatArr
    },
    // 是否是当前月
    isCurrentMonth(date) {
      let { year: currentYear, month: currentMonth } = utils.getNewDate(utils.getDate(this.time.year, this.time.month, 1));
      let { year, month } = utils.getNewDate(date);
      return currentYear == year && currentMonth == month
    },
    // 是否是今天 
    isCurrentDay(date) {
      let { year: currentYear, month: currentMonth, day: currentDay } = utils.getNewDate(new Date());
      let { year, month, day } = utils.getNewDate(date);
      return currentYear == year && currentMonth == month && currentDay == day;
    },
    // 上一个月
    handlePrevMonth() {
      let prevMonth = utils.getDate(this.time.year, this.time.month, 1);
      prevMonth.setMonth(prevMonth.getMonth() - 1);
      this.time = utils.getNewDate(prevMonth);
      this.headOptions.date = `${utils.englishMonth(this.time.month)} ${this.time.year}`;
      this.visibleCalendar()
      this.formtime = []
      this.$emit('handlePrevMonth');
    },
    // 下一个月
    handleNextMonth() {
      let nextMonth = utils.getDate(this.time.year, this.time.month, 1);
      nextMonth.setMonth(nextMonth.getMonth() + 1);
      this.time = utils.getNewDate(nextMonth);
      this.headOptions.date = `${utils.englishMonth(this.time.month)} ${this.time.year}`;
      this.visibleCalendar()
      this.formtime = []
      this.$emit('handleNextMonth');
    },
    // 点击回到今天
    handleToday() {
      this.time = utils.getNewDate(new Date());
      this.headOptions.date = `${utils.englishMonth(this.time.month)} ${this.time.year}`;
      this.visibleCalendar()
      this.formtime = []
      this.$emit('handleToday');
    },
    // 点击某一天
    handleClickDay(item, index) {
      this.$forceUpdate();
      this.$emit('handleClickDay', item);

      if (this.pricetype != 3) {
        this.calendarList.map(x => {
          x.clickDay = false;
        });
        this.$set(item, 'clickDay', true);
        this.index = index
      } else {//多选的情况
        if (this.choosn) {
          if (item.clickDay) {
            this.$set(item, 'clickDay', false);
            this.indexList.splice(this.indexList.findIndex(e => e == index), 1)
          } else {
            this.$set(item, 'clickDay', true);
            this.indexList.push(index)
          }
        }

      }
    },
    handleChange() {

    },
    //单个日期设置价格
    setOnePrice() {
      this.calendarList[this.index].price = this.priceForm.priceA
      this.calendarList[this.index].countPrice = this.priceForm.countPriceA
      this.calendarList[this.index].num = this.priceForm.peopleNum - this.priceForm.orderNum

    },
    //日期范围设置价格
    setRangePrice() {
      if (this.formtime && this.formtime.length > 1) {
        //获取起止日期
        let startDay = new Date(this.formtime[0]).getDate()
        let endDay = new Date(this.formtime[1]).getDate()
        this.calendarList.forEach(ele => {
          if (ele.day >= startDay && ele.day <= endDay && new Date(ele.date).getMonth() == this.time.month) {
            ele.price = this.priceForm.priceA
            ele.countPrice = this.priceForm.countPriceA
            ele.num = this.priceForm.peopleNum - this.priceForm.orderNum
          }
        })
        this.formtime = []
      } else {
        alert('请先选择日期范围')
      }

    },
    //多个日期设置价格
    setPrice() {
      this.indexList.forEach(ele => {
        this.calendarList[ele].price = this.priceForm.priceA
        this.calendarList[ele].countPrice = this.priceForm.countPriceA
        this.calendarList[ele].num = this.priceForm.peopleNum - this.priceForm.orderNum
      })
    },
    chooseDate() {
      this.choosn = true
    }
  },
  created() {
    this.visibleCalendar();
    this.calendarType = this.options.calendarType;
  }
}
</script>

<style lang="less">
.content {
  display: flex;
}
.cc-calendar {
  flex: 1;
  padding: 23px 30px 30px;
  width: 100%;
  height: 100%;
  background: #f9fafc;
  box-sizing: border-box;
  .calendar-week {
    width: 100%;
    height: 46px;
    line-height: 46px;
    border: 1px solid #e4e7ea;
    border-right: none;
    .week-item {
      float: left;
      width: 14.285%;
      text-align: center;
      font-size: 14px;
      color: #424953;
      border-right: 1px solid #e4e7ea;
      font-weight: 600;
    }
  }
  .calendar-view {
    width: 100%;
    border-left: 1px solid #e4e7ea;
    .date-view {
      float: left;
      width: 14.285%;
      height: 100px;
      border-right: 1px solid #e4e7ea;
      border-bottom: 1px solid #e4e7ea;
      cursor: pointer;
      .date-day {
        padding: 8px 8px 0;
        display: block;
        width: 100%;
        font-size: 14px;
        color: #7f8794;
      }
      .calendar-num {
        margin-top: 6px;
        display: block;
        width: 100%;
        text-align: center;
        font-size: 30px;
        color: #424953;
      }
    }
    .opacity-class {
      opacity: 0.5;
    }
    .month-class {
      background-image: linear-gradient(
        45deg,
        rgba(000, 000, 000, 0.03) 25%,
        transparent 25%,
        transparent 50%,
        rgba(000, 000, 000, 0.03) 50%,
        rgba(000, 000, 000, 0.03) 75%,
        transparent 75%,
        transparent
      );
      background-size: 20px 20px;
    }
    .todayBg {
      background: #fffdef;
    }
    .handleDay {
      background: #409eff !important;
      .date-day {
        color: #bccfff !important;
      }
      .productNum {
        color: #bccfff !important;
      }
      .calendar-num {
        color: #fff !important;
      }
    }
  }
}
.productNum,
.productPrice,
.productCountPrice {
  text-align: center;
  line-height: 20px;
}
.productCountPrice {
  color: #7f00ff;
}
.productPrice {
  color: #009900;
}

.productNum {
  color: #7f8794;
}
.formDiv {
  width: 425px;
  border: 1px solid #ccc;
  padding: 23px 20px 30px;
  font-size: 16px;
  color: #333;
}
.formContent {
  display: flex;
  justify-content: space-evenly;
  margin-top: 15px;
}
.formContent > div {
  width: 88px;
  text-align: center;
}
.formFooter {
  margin-top: 200px;
}
.formFooter > div {
  margin-bottom: 16px;
}
.btnContent {
  text-align: center;
}
</style>