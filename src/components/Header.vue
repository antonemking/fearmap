<template>
  <div class="header">
    <a href="/" class="logo">FearMap</a>
    <a href="#" class="link">Fedback</a>
  </div>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style>
.header{
  background: #fff;
  height: 60px;
  padding: 0px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  -webkit-box-shadow: 0 3px 5px rgba(57, 63, 72, 0.3);
  -moz-box-shadow: 0 3px 5px rgba(57, 63, 72, 0.3);
  box-shadow: 0 3px 5px rgba(57, 63, 72, 0.3);
  position: sticky;
}
.logo{
  font-weight: 600;
  font-size: 20px;
}
</style>