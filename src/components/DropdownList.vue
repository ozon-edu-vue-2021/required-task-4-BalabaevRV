<template>
  <div class="input__group">
    <label :for="idInput" class="input__label">{{ labelInput }}</label>
    <input
      :id="idInput"
      @focus="isDropdownOpen = true"
      class="input__field"
      @input="debouncedSearch"
      v-model="currentInput"
    />
    <ul
      v-if="isDropdownOpen"
      class="input__dropdown"
      v-click-outside="closeDropDown"
    >
      <li
        v-for="value in sortList"
        :key="value.id"
        :id="value.id"
        @click="setValue"
      >
        {{ value[field] }}
      </li>
    </ul>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import { debounce } from "@/utils.js";

export default {
  props: {
    labelInput: {
      default: "Подпись поля",
      type: String,
    },
    emptyValue: {
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
      debouncedSearch: debounce(this.filterList, 500),
      currentInput: "",
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
      const currentId = e.target.getAttribute("id");
      this.currentItem = this.listItems.find((item) => item.id == currentId);
      this.value = currentId;
      this.isDropdownOpen = false;
      this.currentInput = this.currentItem[this.field];
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
  },
};
</script>

<style scoped></style>
