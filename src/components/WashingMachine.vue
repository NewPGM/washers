<template>
  <div :class="['washing-machine', { 'occupied': machine.status === 'ไม่ว่าง' }]">
    <img :src="machine.image" alt="Washing Machine" width="100" />
    <h3>{{ machine.name }}</h3>
    <p>ราคา: {{ machine.price }} บาท</p>
    <div v-if="waiting">
      <p>กำลังซัก...</p>
      <p>เวลาที่เหลือ: {{ countdown }} วินาที</p>
    </div>
    <div v-else>
      <p>สถานะ: {{ machine.status }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WashingMachine',
  props: {
    machine: {
      type: Object,
      required: true
    },
    countdown: {
      type: Number,
      default: null
    },
    waiting: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped>
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
</style>
