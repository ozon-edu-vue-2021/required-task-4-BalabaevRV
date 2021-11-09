<template>
  <div class="form">
    <h2 class="groupTitle">Личные данные</h2>
    <div class="inputsGroup">
      <div class="inputsGroup__row">
        <text-input
          idInput="surname"
          labelInput="Фамилия"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.RUSSIA_WORDS"
        />
        <text-input
          idInput="firstName"
          labelInput="Имя"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.RUSSIA_WORDS"
        />
        <text-input
          idInput="middleName"
          labelInput="Отчество"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.RUSSIA_WORDS"
        />
      </div>
      <div class="inputsGroup__row">
        <date-input
          idInput="birthday"
          labelInput="Дата рождения"
          @changeDate="changeInputField"
          :checkDate="checkDate"
        ></date-input>
        <text-input
          idInput="email"
          labelInput="E-mail"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.EMAIL"
        />
      </div>
      <radiobutton-group
        idInput="gender"
        labelInput="Пол"
        :valuesArray="genders"
        @changeInput="changeInputField"
      />
    </div>

    <div class="inputsGroup">
      <h2 class="groupTitle">Паспортные данные</h2>
      <dropdown-list
        idInput="citizenship"
        labelInput="Гражданство"
        :listItems="citizenships"
        emptyValue="Ваше гражданство"
        @changeSelect="changeCitizenship"
        field="nationality"
      />
      <div v-if="russianCitizenships" class="inputsGroup__row">
        <text-input
          idInput="passportSeries"
          labelInput="Серия паспорта"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.ONLY_4_NUMB"
        />
        <text-input
          idInput="passportID"
          labelInput="Номер паспорта"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.ONLY_6_NUMB"
        />
        <date-input
          idInput="dateOfIssue"
          labelInput="Дата выдачи"
          @changeDate="changeInputField"
        ></date-input>
      </div>
      <div v-else-if="info.citizenship">
        <div class="inputsGroup__row">
          <text-input
            idInput="latSurname"
            labelInput="Фамилия на латинице"
            @changeInputField="changeInputField"
            :validationRule="validataionRules.ENGLISH_WORDS"
          />
          <text-input
            idInput="latFirstName"
            labelInput="Имя на латинице"
            @changeInputField="changeInputField"
            :validationRule="validataionRules.ENGLISH_WORDS"
          />
          <text-input
            idInput="passportIDForeign"
            labelInput="Номер паспорта"
            @changeInputField="changeInputField"
            :validationRule="validataionRules.ONLY_NUMB"
          />
        </div>
        <div class="inputsGroup__row">
          <dropdown-list
            idInput="countryOfIssue"
            labelInput="Страна выдачи"
            :listItems="citizenshipsNoRussian"
            emptyValue="Выберите страну"
            field="nationality"
            @changeSelect="changeInputField"
          />
          <dropdown-list
            idInput="typeOfPassport"
            labelInput="Тип паспорта"
            :listItems="typeOfPassport"
            emptyValue="Тип паспорта"
            field="type"
            @changeSelect="changeInputField"
          />
        </div>
      </div>
    </div>

    <div class="inputsGroup">
      <radiobutton-group
        idInput="changeName"
        labelInput="Меняли ли фамилию или имя?"
        :valuesArray="yesno"
        @changeInput="toggleChangeName"
      />
      <div v-if="info.changeName" class="inputsGroup__row">
        <text-input
          idInput="oldSurname"
          labelInput="Фамилия"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.RUSSIA_WORDS"
        />
        <text-input
          idInput="oldFirstName"
          labelInput="Имя"
          @changeInputField="changeInputField"
          :validationRule="validataionRules.RUSSIA_WORDS"
        />
      </div>
    </div>
    <primary-button sign="Отправить" @clickPrimaryButton="doSubmit" />
  </div>
</template>

<script>
import TextInput from "./TextInput.vue";
import DateInput from "./DateInput.vue";
import RadiobuttonGroup from "./RadiobuttonGroup.vue";
import DropdownList from "./DropdownList.vue";
import PrimaryButton from "./PrimaryButton.vue";
import citizenships from "@/assets/data/citizenships.json";
import typeOfPassport from "@/assets/data/passport-types.json";
import { validataionRules } from "@/constants/validationRules.js";
import { NUM_OF_RUSSIAN } from "@/constants/numOfRussian.js";

export default {
  components: {
    TextInput,
    DateInput,
    RadiobuttonGroup,
    DropdownList,
    PrimaryButton,
  },
  data() {
    return {
      checkDate: true,
      genders: [
        {
          name: "Мужской",
          slug: "male",
          checked: false,
        },
        {
          name: "Женский",
          slug: "female",
          checked: false,
        },
      ],
      yesno: [
        {
          name: "нет",
          slug: "false",
          checked: true,
        },
        {
          name: "да",
          slug: "true",
          checked: false,
        },
      ],
      citizenships: [],
      typeOfPassport: [],
      changeName: false,
      info: {
        surname: "",
        firstName: "",
        middleName: "",
        birthday: "",
        email: "",
        gender: "",
        citizenship: "",
        passportSeries: "",
        passportID: "",
        passportIDForeign: "",
        dateOfIssue: "",
        latSurname: "",
        latFirstName: "",
        typeOfPassport: "",
        countryOfIssue: "",
        changeName: false,
        oldSurname: "",
        oldFirstName: "",
      },
      errorsArray: [],
      validataionRules: validataionRules,
      foreignPassportField: [
        "latSurname",
        "latFirstName",
        "passportIDForeign",
        "countryOfIssue",
        "typeOfPassport",
      ],
      RussianPassportField: ["passportID", "passportSeries", "dateOfIssue"],
    };
  },
  computed: {
    citizenshipsNoRussian: function () {
      return this.citizenships.slice(1);
    },
    russianCitizenships: function () {
      return this.info.citizenship === NUM_OF_RUSSIAN;
    },
  },
  methods: {
    toggleChangeName(data) {
      this.info.changeName = data.value === "true";
    },
    changeCitizenship(data) {
      if (data.value === NUM_OF_RUSSIAN) {
        this.cleanInfo(this.foreignPassportField);
      } else {
        this.cleanInfo(this.RussianPassportField);
      }
      this.changeInputField(data);
    },
    cleanInfo(currentArray) {
      currentArray.forEach((element) => {
        (this.info[element] = ""),
          this.errorsArray.splice(this.errorsArray.indexOf(element), 1);
      });
    },
    changeInputField(data) {
      this.info[data.id] = data.value;
      this.changeErrorsArray(data.id, data.haveError);
    },
    changeErrorsArray(field, haveError) {
      if (this.errorsArray.includes(field)) {
        if (haveError) {
          return;
        } else {
          this.errorsArray.splice(this.errorsArray.indexOf(field), 1);
        }
      } else {
        if (haveError) {
          this.errorsArray.push(field);
        } else {
          return;
        }
      }
    },
    doSubmit() {
      if (this.errorsArray.length > 0) {
        alert("Заполните корректно поля");
      } else {
        alert("Данные отправлены");
        console.log(JSON.stringify(this.info));
      }
    },
  },
  mounted() {
    this.citizenships = citizenships;
    this.typeOfPassport = typeOfPassport;
  },
};
</script>

<style scoped>
.form {
  width: 680px;
}

.inputsGroup {
  margin-bottom: 40px;
}

.inputsGroup__row {
  display: flex;
  justify-content: flex-start;
  gap: 20px;
}

.groupTitle {
  margin-bottom: 10px;
}
</style>

<style>
.input__label {
  display: block;
  padding-left: 5px;
  margin: 0;
  margin-bottom: 5px;
  font-size: 18px;
}

.input__field {
  height: 40px;
  border-radius: 5px;
  border: 2px solid #929cb1;
  width: 220px;
  font-size: 18px;
  padding-left: 5px;
}

.input__field--error {
  border: 2px solid #dd3d0d;
}

.input__field:hover,
.input__field:active,
.input__field:focus {
  border: 2px solid #234f91;
  outline: none;
}

.input__group {
  margin-bottom: 20px;
  position: relative;
}

.input__dropdown {
  list-style: none;
  background-color: #ffffff;
  padding-top: 10px;
  padding-left: 0;
  border: 1px solid #929cb1;
  border-radius: 10px;
  max-width: 250px;
  margin-top: 5px;
  position: absolute;
  width: 100%;
  height: 300px;
  overflow-y: scroll;
  z-index: 2;
}

.input__dropdown li {
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 10px;
}

.input__dropdown li:hover {
  background-color: #234f91;
  cursor: pointer;
  color: #ffffff;
}
</style>
