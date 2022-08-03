<template>
  <div>
    {{time}}
  </div>
  <hr>

  <span>Attempt #{{currentAttempt}}</span>
  <br>
  <span>expectation: {{expectation}}%</span>
  <br>
  <button @click="isTimerMoving = !isTimerMoving">Timer:{{isTimerMoving}}</button>
  <br>
  <button @click="startCounting()">start counting </button>
  <br>
  <!-- <button @click="currentAttempt+= 100">Maggic </button> -->
  <!-- &nbsp;&nbsp;&nbsp; -->
  <button @click="reCal()">recal</button>
  <input type="number" v-model="tempNum">
  <hr>

  <div class="table">
    <table style="margin-left: -100px;">
      <thead>
        <tr>
          <th class="primary" scope="col">#</th>
          <th scope="col">%</th>
        </tr>
      </thead>
      <tbody>
        <template  v-for="(row,index) in tableData" :key="index">

          <tr v-if="row<=5000">
            <th scope="row">{{row}}</th>
            <td  :class="[currentAttempt >=row && currentAttempt< row+250 ? 'here' : '']">{{cul(row)}}%</td>
          </tr>

        </template>
      
      </tbody>
    </table>

    <table style="margin-left: 20px;">
      <thead>
        <tr>
          <th class="primary" scope="col">#</th>
          <th scope="col">%</th>
        </tr>
      </thead>
      <tbody>
        <template  v-for="(row,index) in tableData" :key="index">

          <tr v-if="row>5000 && row<=10000">
            <th scope="row">{{row}}</th>
            <td :class="[currentAttempt >=row && currentAttempt< row+250 ? 'here' : '']">{{cul(row)}}%</td>
          </tr>

        </template>
      
      </tbody>
    </table>

    <table style="margin-left: 20px;">
      <thead>
        <tr>
          <th class="primary" scope="col">#</th>
          <th scope="col">%</th>
        </tr>
      </thead>
      <tbody>
        <template  v-for="(row,index) in tableData" :key="index">

          <tr v-if="row>10000">
            <th scope="row">{{row}}</th>
            <td :class="[currentAttempt >=row && currentAttempt< row+250 ? 'here' : '']">{{cul(row)}}%</td>
          </tr>

        </template>
      
      </tbody>
    </table>
  </div>
  
</template>

<script>
// const stopwatch = useStopwatch(autoStart);

// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },

  data(){
    return{
      
      // idealGoal: 1111,
      notShinyChance: 0.99987792968,
      wholeSpan: 15,
      currentAttempt: 0,

      tableData: [],
      hours: 0,
      minutes: 0,
      seconds: 0,

      isTimerMoving: true,
      tempNum: 0,

    }
  },
  methods:{
    cul(theNum){
      // console.log(this.shinyChance)
      let num1 = this.notShinyChance ** theNum
      // return num1
      num1 = (1 -num1) * 100
      num1 = Math.round(num1 * 10) / 10
      

      return num1
    },

    sleep(ms) {
      return new Promise((resolve) => {
        setTimeout(resolve, ms);
      });
    },

    async startCounting(){
      let count = 0
      while(this.isTimerMoving){
        await this.sleep(1000);
        this.seconds++
        console.log(this.isTimerMoving)
        count++
        if(count == this.wholeSpan){
          count = 0
          this.currentAttempt++
        }
      }
    },

    convertNumToStr(num){
      if(num == 0){
        return '00'
      }else if(num <10){
        return `0${num}`
      }else{
        return num
      }
    },

    reCal(){
      this.currentAttempt = this.tempNum
      let theSeconds = this.currentAttempt * this.wholeSpan
      console.log(theSeconds)
      let theMinutes = 0
      let theHours = 0

      while(theSeconds>=3600){
        theHours++
        theSeconds-= 3600
      }

      while(theSeconds>=60){
        theMinutes++
        theSeconds-= 60
      }

      console.log(`h:${theHours}, m: ${theMinutes}, s:${theSeconds}`)
      this.hours = theHours
      this.minutes = theMinutes
      this.seconds = theSeconds
      console.log(typeof(theSeconds))
    }
    


  },
  watch:{
    seconds(){
      if(this.seconds < 60) return
      this.seconds =0 
      this.minutes++
    },

    minutes(){
      if(this.minutes < 60) return
      this.minutes =0 
      this.hours++
    },

    currentAttempt(){
      localStorage.currentAttempt = this.currentAttempt
    },


  },
  computed:{
    expectation(){
      let num1 = this.notShinyChance ** this.currentAttempt
      // return num1
      num1 = (1 -num1) * 100
      return Math.round(num1 * 100) / 100
    },
    time(){
      // this.convertNumToStr()
      return `${this.convertNumToStr(this.hours)}:${this.convertNumToStr(this.minutes)}:${this.convertNumToStr(this.seconds)}`
    },

  },
  mounted(){
    if(!localStorage.currentAttempt){
      this.currentAttempt =localStorage.currentAttempt
    }else{
      this.currentAttempt =localStorage.currentAttempt
      this.tempNum = this.currentAttempt
      console.log(this.currentAttempt)
      this.reCal()
    }
    // let count =0 
    let num = 250

    while(num <= 15000){
      this.tableData.push(num)
      num+= 250
    }

    // console.log(this.tableData)
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.table{
  width: 50%;
  margin: 0 auto;
  display: flex;
}

table {
  text-align: left;
  position: relative;
  border-collapse: collapse; 
  background-color: #f6f6f6;
}/* Spacing */
td, th {
  border: 1px solid #999;
  padding: 10px;
}
th {
  background: brown;
  color: white;
  border-radius: 0;
  position: sticky;
  top: 0;
  padding: 10px;
}
.primary{
  background-color: #000000
}

tfoot > tr  {
  background: black;
  color: white;
}
.here {
  background-color: #ffc107;
}



</style>
