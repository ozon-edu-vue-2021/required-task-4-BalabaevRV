<template>
  <div class="input__group">
    <label :for="idInput" class="input__label">{{ labelInput }}</label>
    <date-picker
      v-model="value"
      :input-class="inputClasses"
      placeholder="дд.мм.гггг"
      format="DD.MM.YYYY"
      :input-attr="dateAttr"
      @change="changeDate"
    ></date-picker>
  </div>
</template>

<script>
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";
import "vue2-datepicker/locale/ru";

export default {
  components: {
    DatePicker,
  },
  props: {
    labelInput: {
      default: "Подпись поля",
      type: String,
    },
    idInput: {
      default: "",
      type: String,
    },
    checkDate: {
      default: false,
      type: Boolean,
    },
  },
  data() {
    return {
      value: null,
      dateAttr: {
        name: this.idInput,
        id: this.idInput,
      },
      haveError: false,
      inputClasses: "input__field",
    };
  },
  methods: {
    changeDate() {
      if (this.checkDate && this.value) {
        // const dateArray = this.value.split(".");
        // const dateString =
        //   dateArray[1] + "." + dateArray[0] + "." + dateArray[2];
        // const chooseDate = new Date(dateString);
        // const currentDate = new Date().setHours(0, 0, 0, 0);
        if (new Date(this.value) <= new Date()) {
          this.haveError = false;
          this.inputClasses = "input__field";
        } else {
          this.haveError = true;
          this.inputClasses = this.inputClasses + " input__field--error";
        }
      }
      this.$emit("changeDate", {
        value: this.value,
        id: this.idInput,
        haveError: this.haveError,
      });
    },
  },
};
</script>

<style>
.input__group {
  width: 220px;
}
</style>
