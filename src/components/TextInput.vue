<template>
  <div class="input__group">
    <label :for="idInput" class="input__label">{{ labelInput }}</label>
    <input
      type="text"
      :id="idInput"
      :name="idInput"
      class="input__field"
      :class="['input__field', { 'input__field--error': haveError }]"
      @change="changeInputField"
      autocomplete="off"
    />
  </div>
</template>

<script>
export default {
  props: {
    labelInput: {
      default: "Подпись поля",
      type: String,
    },
    idInput: {
      default: "",
      type: String,
    },
    validationRule: {
      default: "",
      type: RegExp,
    },
  },
  data() {
    return {
      haveError: false,
    };
  },
  methods: {
    changeInputField(e) {
      const newValue = e.target.value;
      if (this.validationRule) {
        const match = newValue.match(this.validationRule);
        if (match) {
          this.haveError = false;
        } else {
          this.haveError = true;
        }
      }
      this.emitChange(newValue, e.target.id);
    },
    emitChange(newValue, id) {
      this.$emit("changeInputField", {
        value: newValue,
        id: id,
        haveError: this.haveError,
      });
    },
  },
};
</script>

<style scoped></style>
