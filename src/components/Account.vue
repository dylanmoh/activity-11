<template>
  <div class="budget projector">
    <h1>Budget Projector</h1>
    <form v-on:submit.prevent="calculateGoal">
      <div class="form-item-wrap">
      <label>Enter Name</label>
      <input type="text" v-model="name">
      </div>
      <div class="form-item-wrap">
      <label>Enter Current Balance</label>
      <input type="text" v-model="currentBalance">
      </div>
      <div class="form-item-wrap">
      <label>Enter Hours Worked a Week</label>
      <input type="text" v-model="hours">
      </div>
      <div class="form-item-wrap">
      <label>Enter Pay Per Hour</label>
      <input type="text" v-model="pay">
      </div>
      <div class="form-item-wrap">
      <label>Enter Monthly Expenses</label>
      <input type="text" v-model="expenses">
      </div>
      <div class="form-item-wrap">
      <label>Likelyhood of buying tacos each day (1-10)</label>
      <input type="text" v-model="taco">
      </div>
      <div class="form-item-wrap">
      <label>Enter Budget Goal</label>
      <input type="text" v-model="goal">
      </div>
      <button type="submit">Submit</button>
    </form>
    <div class="results-wrap" v-for="item in filteredComplete" :key="item.title">
      <label v-if="item.status">{{name}} you will reach your goal in {{item.totalDays}} days</label>
      <label v-if="!item.status">{{name}} your expenses are too high and you need to make more money</label>
    </div>
    <ul>
      <li v-for="item in filteredTransactions" :key="item.title">
        <label v-if="item.type == 'deposit'" class="deposit"><span>{{item.description}}</span><span>Day:{{ item.day }}</span><span>Amount:${{item.amount}}</span><span class="balance">Balance After Transaction:${{item.balance}}</span></label>
        <label v-if="item.type == 'withdraw'" class="withdraw"><span>{{item.description}}</span><span>Day:{{ item.day }}</span><span>Amount:${{item.amount}}</span><span class="balance">Balance After Transaction:${{item.balance}}</span></label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data () {
    return {
      name: '',
      currentBalance: 0,
      expenses: 0,
      hours: 0,
      pay: 0,
      goal: 0,
      totalDays: [],
      transactions: [],
      days: 0,
      taco: 0,
      complete: true,
      finished: []
    }
  },
  computed: {
    filteredComplete: function () {
      return this.finished
    },
    filteredTransactions: function () {
      return this.transactions
    }
  },
  methods: {
    calculateGoal: function () {
      var money = this.currentBalance
      this.complete = true
      this.finished = []
      this.transactions = []
      this.days = 0
      var transaction = 0
      while (money < this.goal) {
        if (money < 0) {
          this.complete = false
          break
        }
        this.days += 1
        if (((Math.floor(Math.random() * ((11 - this.taco) - 1 + 1)) + 1) % (11 - this.taco)) === 0) {
          money = parseFloat(money) - 10
          this.transactions.push({ type: 'withdraw', day: this.days, amount: 20, balance: money, description: 'Tacos' })
        }
        if ((this.days % 7) === 0) {
          transaction = this.pay * this.hours
          money = parseFloat(money) + parseFloat(transaction)
          this.transactions.push({ type: 'deposit', day: this.days, amount: transaction, balance: money, description: 'Paycheck' })
        }
        if ((this.days % 30) === 0) {
          money = parseFloat(money) - parseFloat(this.expenses)
          this.transactions.push({ type: 'withdraw', day: this.days, amount: this.expenses, balance: money, description: 'Rent' })
        }
      }
      this.finished.push({status: this.complete, totalDays: this.days})
    }
  }
}
</script>

<style scoped>
 h1 {
    text-align: center;
    color: white;
 }
 form {
     padding: 10px;
    border: solid lightgray;
    background: white;
 }
 .form-item-wrap {
   margin: 10px 0;
 }

 ul {
     list-style: none;
 }

 li {
     background: #f3f3f3;
     min-height: 30px;
     margin-bottom: 10px;
     font-size: 1em;
     display: flex;
     align-items: center;
 }

 li span {
   width: 15%;
   display: block;
   text-align: center;
   float: left;
 }

 li .balance {
   width: 55%;
 }

 ul label {
     width: 100%;
     padding: 10px 0;
     font-size: 14px;
 }

 /* Form */

 input[type=text] {
     font-size: 1em;
 }

 button {
     font-family: 'Arvo';
     font-size: 1em;
 }

 .results-wrap {
    text-align: center;
    color: white;
    margin: 30px;
    font-size: 25px;
 }
 .withdraw {
  border: solid red;
 }
 .deposit {
  border: solid green;
 }
</style>
