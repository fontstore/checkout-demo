<template>
  <a class="button" :class="{ __disabled: disabled }"><slot /></a>
</template>

<script>
export default {
  name: "VButton",
  props: {
    disabled: { type: Boolean, default: false },
  },
  components: {},
};
</script>

<style scoped>
.button {
  padding: 0px 12px;
  display: inline-block;
  background: #1a1a1b;
  color: #fff;
  font-family: system-ui;
  line-height: 34px;
  font-size: 12px;
  text-transform: lowercase;
  padding: 0px 12px 3px 12px;
  border-radius: 2px;
  cursor: pointer;
}
.button.__disabled {
  opacity: 0.5;
  pointer-events: none;
}
</style>
