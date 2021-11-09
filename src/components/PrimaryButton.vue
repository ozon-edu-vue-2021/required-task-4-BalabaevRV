<template>
  <button class="btnPrimary" @click="clickButton">{{ sign }}</button>
</template>

<script>
export default {
  props: {
    sign: {
      default: "Подпись кнопки",
      type: String,
    },
  },
  data() {
    return {};
  },
  methods: {
    clickButton() {
      this.$emit("clickPrimaryButton");
    },
  },
};
</script>

<style scoped>
.btnPrimary {
  padding: 15px 30px;
  background-color: #518be2;
  border: none;
  border-radius: 10px;
  margin-bottom: 40px;
  color: #fff;
  font-size: 18px;
  cursor: pointer;
}

.btnPrimary:active {
  background-color: #234f91;
}
</style>
