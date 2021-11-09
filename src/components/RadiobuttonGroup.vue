<template>
  <div>
    <p class="radibutton__title">{{ labelInput }}</p>
    <div class="radibutton__group">
      <div
        v-for="value in valuesArray"
        :key="value.slug"
        class="radibutton__item"
      >
        <input
          type="radio"
          :id="idInput + '-' + value.slug"
          :name="idInput"
          class="radibutton__input"
          :value="value.slug"
          v-model="currentValue"
          :checked="value.checked"
        />
        <span class="radibutton__checkmark"></span>
        <label :for="idInput + '-' + value.slug" class="radibutton__label">{{
          value.name
        }}</label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    labelInput: {
      type: String,
      default: "Подпись поля",
    },
    idInput: {
      type: String,
      default: "",
    },
    valuesArray: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      currentValue: ""
    };
  },
  watch: {
    currentValue: function (newVal) {
      this.$emit("changeInput", { value: newVal, id: this.idInput });
    }
  },
};
</script>

<style scoped>
.radibutton__group {
  display: flex;
}

.radibutton__item {
  display: flex;
  position: relative;
  align-items: center;
  margin-right: 20px;
  margin-bottom: 10px;
  height: 20px;
}

.radibutton__input {
  position: absolute;
  overflow: hidden;
  clip: rect(0 0 0 0);
  height: 1px;
  width: 1px;
}

.radibutton__label,
.radibutton__title {
  font-size: 18px;
  z-index: 1;
  margin: 0;
  cursor: pointer;
}

.radibutton__title {
  margin-bottom: 5px;
}

.radibutton__label {
  padding-left: 25px;
}

.radibutton__checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  border-radius: 50%;
  background-color: #fff;
  border: 2px solid #929cb1;
  z-index: 0;
}

.radibutton__input:checked ~ .radibutton__checkmark {
  border: 5px solid #518be2;
}
</style>
