<script setup>
import CheckoutDemo from "./components/CheckoutDemo.vue";
</script>

<template>
  <div class="layout">
    <div class="layout__lc">&nbsp;</div>
    <div class="layout__rc">
      <div class="content">
        <h1>Checkout demo</h1>
        <CheckoutDemo />
      </div>
    </div>
  </div>
</template>

<style scoped>
.layout {
  width: 100%;
  height: 100%;
  display: flex;
}
.layout__lc,
.layout__rc {
  height: 100%;
  flex: 1;
}
.layout__lc {
  max-width: 300px;
  background: #eaeaea;
}
.layout__rc {
  background: #f7f7f7;
}
.content {
  width: 460px;
  margin: 20px auto;
}
h1 {
  color: #1a1a1b;
  font-family: system-ui, -apple-system;
  font-size: 20px;
  font-style: normal;
  font-weight: 300;
  line-height: 30px;
}
</style>
