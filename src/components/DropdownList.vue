<template>
  <div class="input__group">
    <label :for="idInput" class="input__label">{{ labelInput }}</label>
    <input
      :id="idInput"
      @focus="isDropdownOpen = true"
      class="input__field"
      @input="debouncedSearch"
      ref="currentInput"
    />
    <ul
      v-if="isDropdownOpen"
      class="input__dropdown"
      v-click-outside="closeDropDown"
    >
      <li
        v-for="value in sortList"
        :key="value.id"
        :currentId="value.id"
        @click="setValue"
      >
        {{ value[field] }}
      </li>
    </ul>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";

export default {
  created() {
    this.debouncedSearch = this.debounce(this.filterList, 500);
  },
  props: {
    labelInput: {
      default: "Подпись поля",
      type: String,
    },
    EmptyValue: {
      default: "",
      type: String,
    },
    idInput: {
      default: "",
      type: String,
    },
    field: {
      default: "",
      type: String,
    },
    listItems: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  directives: {
    ClickOutside,
  },
  data() {
    return {
      isDropdownOpen: false,
      currentItem: {},
      filterValue: "",
    };
  },
  computed: {
    sortList: function () {
      let List;
      if (this.filterValue) {
        List = this.listItems.filter((item) =>
          item[this.field]
            .toUpperCase()
            .includes(this.filterValue.toUpperCase())
        );
      } else {
        List = this.listItems;
      }
      return [...List].sort((a, b) =>
        a[this.field].localeCompare(b[this.field])
      );
    },
  },
  methods: {
    setValue(e) {
      const currentId = e.target.getAttribute("currentId");
      this.currentItem = this.listItems.find((item) => item.id == currentId);
      this.value = currentId;
      this.isDropdownOpen = false;
      this.$refs.currentInput.value = this.currentItem[this.field];
      this.$emit("changeSelect", { value: currentId, id: this.idInput });
    },
    closeDropDown(e) {
      if (e.target.id === this.idInput) {
        return;
      } else {
        this.isDropdownOpen = false;
      }
    },
    filterList(e) {
      this.filterValue = e.target.value;
      this.isDropdownOpen = true;
    },
    debounce(f, t) {
      const calls = {};
      return function (args) {
        let previousCall = calls.lastCall;
        calls.lastCall = Date.now();
        if (previousCall && calls.lastCall - previousCall <= t) {
          clearTimeout(calls.lastCallTimer);
        }
        calls.lastCallTimer = setTimeout(() => f(args), t);
      };
    },
  },
};
</script>

<style scoped></style>
