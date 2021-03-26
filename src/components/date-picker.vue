<template>
  <div class="content">
    {{ pricetype }}
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
            v-model="value1"
            type="daterange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
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
              :min="1"
              :max="10"
            ></el-input-number>
          </div>
          <div>最多已定：23</div>
        </div>
        <div class="btnContent">
          <el-button type="primary" size="medium">应用</el-button>
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
            <el-input v-model="input" placeholder="请输入内容"></el-input>
          </div>
          <div>
            <el-input v-model="input" placeholder="请输入内容"></el-input>
          </div>
          <div>
            <el-input v-model="input" placeholder="请输入内容"></el-input>
          </div>
        </div>
        <div class="formContent">
          <div>结算价</div>
          <div>
            <el-input v-model="input" placeholder="请输入内容"></el-input>
          </div>
          <div>
            <el-input v-model="input" placeholder="请输入内容"></el-input>
          </div>
          <div>
            <el-input v-model="input" placeholder="请输入内容"></el-input>
          </div>
        </div>
        <div class="formFooter">
          <div>库存</div>
          <div>
            计划人数：<el-input-number
              v-model="num"
              controls-position="right"
              @change="handleChange"
              :min="1"
              :max="10"
            ></el-input-number>
          </div>
          <div>已选：23天</div>
        </div>
        <div class="btnContent">
          <el-button type="primary" size="medium">选择日历</el-button>
          <el-button type="primary" size="medium">应用</el-button>
          <el-button size="medium">取消</el-button>
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
      input: ''
    }
  },
  watch: {
    options: {
      handler(newValue, oldValue) {
        this.pricetype = newValue.pricetype
        //父组件param对象改变会触发此函数
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
      this.$emit('handlePrevMonth');
    },
    // 下一个月
    handleNextMonth() {
      let nextMonth = utils.getDate(this.time.year, this.time.month, 1);
      nextMonth.setMonth(nextMonth.getMonth() + 1);
      this.time = utils.getNewDate(nextMonth);
      this.headOptions.date = `${utils.englishMonth(this.time.month)} ${this.time.year}`;
      this.visibleCalendar()
      this.$emit('handleNextMonth');
    },
    // 点击回到今天
    handleToday() {
      this.time = utils.getNewDate(new Date());
      this.headOptions.date = `${utils.englishMonth(this.time.month)} ${this.time.year}`;
      this.visibleCalendar()
      this.$emit('handleToday');
    },
    // 点击某一天
    handleClickDay(item, index) {
      this.$forceUpdate();
      this.$emit('handleClickDay', item);
      this.calendarList.map(x => {
        x.clickDay = false;
      });
      this.$set(item, 'clickDay', true);
      this.index = index
    },
    handleChange() {

    },
    //单个日期设置价格
    setOnePrice() {
      console.log(this.priceForm)
      this.calendarList[this.index].price = this.priceForm.priceA
      this.calendarList[this.index].countPrice = this.priceForm.countPriceA
      this.calendarList[this.index].num = this.priceForm.peopleNum - this.priceForm.orderNum

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