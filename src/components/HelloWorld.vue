<template>
  <div class="container">
    <table class="calendar">
      <thead>
        <tr>
          <td>
            <button v-on:click="prevMonth">-</button>
          </td>
          <td colspan="4">
            {{months[month]}} {{year}}
          </td>  
          <td>
            <button v-on:click="nextMonth">+</button>
          </td>
          <td>
            <button v-on:click="lang">Ru/Eng</button>
          </td>    
        </tr>
        <tr class="days_name">
          <td v-for="day in this.days" :key="day">{{day}}</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(week, index) in monthDays()" :key="index">
          <td v-for="(d, index) in week" :key="index" v-on:click="getDate(d)" class="date">{{ d[0] }}</td>
        </tr>
      </tbody> 
    </table>
    <div>Выбранная дата: {{selectDate}}</div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      year: '',
      month: '',
      daysRu: ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб", "Вс"],
      daysEng: ["Mon", "Tue", "Wen", "Thu", "Fri", "Sat", "Sun"],
      monthsRu: ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"],
      monthsEng: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
      days: [],
      months: [],
      language: '',
      selectDate: '',
    }
    
  },
  methods: {
    prevMonth() {
      this.month--;
      if (this.month < 0) {
        this.month = 11;
        this.year--;
      }
    },
    nextMonth() {
      this.month++;
      if (this.month > 11) {
        this.month = 0;
        this.year++;
      }
    },
    lang() {
      if (this.language === 'Ru') {
        this.language = 'Eng';
        this.months = [...this.monthsEng];
        this.days = [...this.daysEng];
      } else {
        this.language = 'Ru';
        this.months = [...this.monthsRu];
        this.days = [...this.daysRu];
      }
    },
    monthDays() {
      let daysOfMonth = [];
      let weekNum = 0;
      daysOfMonth[weekNum] = [];
      let lastDay = new Date(this.year, this.month + 1, 0).getDate(); //последний день предыдущего месяца
      for ( let i = 1; i <= lastDay; i++) { //идем ао дням месяца и ищем понедельники
        if (new Date(this.year, this.month, i).getDay() == 1) { //если это понедельник, то новая неделя
          weekNum++;
          daysOfMonth[weekNum] = []; //она пустая
          daysOfMonth[weekNum].push([i, new Date(this.year, this.month, i)]); // кладем туда понедельник
        } else {
          daysOfMonth[weekNum].push([i, new Date(this.year, this.month, i)]);
        }
      }
      if (daysOfMonth[0].length > 0) {
        for (let i = daysOfMonth[0].length; i < 7; i++) {
          daysOfMonth[0].unshift('') //добавляем в начало пустые строки пока не будет 7 элементов в массиве
        }
      }
      return daysOfMonth;
    },
    getDate(date) {
      // let elem = event.target;
      // elem.classList.add('active');
      // console.log(date[1]);
      this.selectDate = date[1];
    }

  },
  mounted() {
    this.year = new Date().getFullYear(),
    this.month = new Date().getMonth(),
    this.language = 'Ru';
    this.months = [...this.monthsRu];
    this.days = [...this.daysRu];
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display:flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.calendar {
  width: 350px;
  margin: 50px 0;
}
.days_name {
  height: 50px;
  background-color: rgb(141, 228, 202);
}
.date:hover {
  cursor: pointer;
}
.active {
  background-color: rgb(141, 228, 202);
}
td {
  width: 50px;
  height: 50px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
