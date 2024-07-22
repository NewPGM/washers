<template>
  <div id="app">
    <h1>ร้านซักผ้า ออนไลน์</h1>
    <div>
      <label for="amount">ใส่จำนวนเงิน:</label><br>
      <input type="number" id="amount" v-model="amount" /><br>
      <button @click="check">ยืนยัน</button>
      <button @click="re">เริ่มใหม่</button>
      <p>จํานวน: {{ amount }} เงิน</p>
    </div>
    <div v-if="availableWasher && !waiting">
      <p>ตอนนี้เครื่องซักผ้าที่ {{ availableWasher.id }} กำลังว่าง</p>
      <button @click="confirm">ยืนยันที่จะใช้เครื่องนี้</button>
    </div>
    <div v-else-if="amountSubmitted && !waiting">
      <p>ตอนนี้เครื่องซักผ้ายังไม่ว่าง</p>
    </div>
    <div v-else>
      <p>{{ message }}</p>
    </div>
    <div>
      <h1>ดูเครื่องซักผ้า</h1>
      <div class="machine-list">
        <WashingMachine
          v-for="machine in washers"
          :key="machine.id"
          :machine="machine"
          :countdown="machine.id === (availableWasher ? availableWasher.id : null) ? countdown : null"
          :waiting="machine.id === (availableWasher ? availableWasher.id : null) ? waiting : false"
        />
      </div>
    </div>
  </div>
</template>

<script>
import WashingMachine from './components/WashingMachine.vue'

export default {
  name: 'App',
  components: {
    WashingMachine
  },
  data () {
    return {
      amount: 0,
      washers: [
        { id: 1, name: 'เครื่องซักผ้าที่ 1', status: 'ว่าง', price: 20, image: 'https://png.pngtree.com/png-clipart/20231016/original/pngtree-washing-machine-illustration-png-image_13323675.png' },
        { id: 2, name: 'เครื่องซักผ้าที่ 2', status: 'ว่าง', price: 20, image: 'https://png.pngtree.com/png-clipart/20231016/original/pngtree-washing-machine-illustration-png-image_13323675.png' },
        { id: 3, name: 'เครื่องซักผ้าที่ 3', status: 'ว่าง', price: 20, image: 'https://png.pngtree.com/png-clipart/20231016/original/pngtree-washing-machine-illustration-png-image_13323675.png' },
        { id: 4, name: 'เครื่องซักผ้าที่ 4', status: 'ว่าง', price: 20, image: 'https://png.pngtree.com/png-clipart/20231016/original/pngtree-washing-machine-illustration-png-image_13323675.png' }
      ],
      availableWasher: null,
      amountSubmitted: false,
      message: '',
      waiting: false,
      countdown: 0,
      countdownInterval: null
    }
  },
  methods: {
    check () {
      if (this.amount === 20) {
        this.findAvailableWasher()
      } else {
        this.message = 'โปรดใส่จำนวนเงิน 20 บาท'
      }
    },
    findAvailableWasher () {
      this.amountSubmitted = true
      this.availableWasher = this.washers.find(washer => washer.status === 'ว่าง')
    },
    confirm () {
      if (this.availableWasher) {
        this.startCountdown()
      } else {
        this.check()
      }
      this.amount = 0
    },
    startCountdown () {
      this.waiting = true
      this.countdown = 60
      const washer = this.washers.find(w => w.id === this.availableWasher.id)
      if (washer) {
        washer.status = 'ไม่ว่าง'
      }
      this.countdownInterval = setInterval(() => {
        this.countdown -= 1
        if (this.countdown <= 0) {
          clearInterval(this.countdownInterval)
          this.countdownInterval = null
          this.updateWasherStatus()
          this.reset()
        }
      }, 1000)
    },
    updateWasherStatus () {
      if (this.availableWasher) {
        const washer = this.washers.find(w => w.id === this.availableWasher.id)
        if (washer) {
          washer.status = 'ว่าง'
        }
        this.availableWasher = null
        this.check()
      }
    },
    reset () {
      this.waiting = false
      this.countdown = 0
      if (this.countdownInterval) {
        clearInterval(this.countdownInterval)
        this.countdownInterval = null
      }
    },
    re () {
      this.amount = 0
      this.amountSubmitted = false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  margin-top: 10px;
}
.washing-machine {
  border: 1px solid #ccc;
  padding: 16px;
  margin: 8px;
  border-radius: 8px;
  position: relative;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}
.washing-machine.occupied {
  border-color: red;
}
.machine-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
