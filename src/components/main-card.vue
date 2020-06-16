<template>
  <div class='card'>
    <p class='title'>{{ cardTitle }}</p>
    <p class='number'>{{ cardNumber.toLocaleString() }}</p>
    <p class='date'>{{ cardDate }}</p>
    <p class='text'>{{ cardText }}</p>
  </div>
</template>

<script>
export default {
  name: 'MainCard',
  props: {
    cardTitle: String,
    cardNumber: Number,
    cardDate: String,
    cardText: String
  }
}
</script>

<style scoped lang="scss">
.card {
  width: 240px;
  background-color: #fff;
  box-shadow: 0 0 2px rgba(0,0,0,0.1);
  border-radius: 5px;
  padding: 5px;
  display: inline-block;
  min-width: 230px;
  margin-right: 10px;
  font-size: 16px;
}
.number {
  font-size: 20px;
}
.date {
  font-size: 15px;
}
.title, .date {
  color: #ccc;
}
.title, .number, .date, .text {
  margin: 10px 0 0 19px;
}
</style>
