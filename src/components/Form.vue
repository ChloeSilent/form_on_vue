<template>
<form class="form" @submit.prevent="processForm">
  <template v-for="item in fields">
    <Select v-if="item.options"
    v-bind:key="item.name"
    v-bind:item="item" v-model="item.value"
    v-on:changedItemData="changeFieldData($event)"
    />
    <Checkbox
    v-else-if="typeof item.value === 'boolean'"
    :key="item.name"
    v-bind:item="item"
    v-model="item.value"
    v-on:changedItemData="changeCheckBoxData($event)"
    />
    <TextInput
    v-else
    v-bind:key="item.name"
    v-bind:item="item"
    v-model="item.value"
    v-on:changedItemData="changeFieldData($event)"
    />
  </template>
  <Button v-on:click="submit(e)" />
</form>
</template>

<script>
import {
  required, minLength,
} from 'vuelidate/lib/validators';

import TextInput from './TextInput.vue';
import Select from './Select.vue';
import Checkbox from './Checkbox.vue';
import Button from './Button.vue';

const errorMessages = {
  required: 'Заполните поле',
  minLength: 'Введено недостаточно символов',
};

export default {
  name: 'Form',
  components: {
    TextInput,
    Select,
    Checkbox,
    Button,
  },
  data() {
    return {
      fields: {
        secondName: {
          name: 'secondName',
          title: 'Фамилия',
          error: false,
          errorMessage: '',
          value: '',

        },
        firstName: {
          name: 'firstName',
          title: 'Имя',
          error: false,
          errorMessage: '',
          value: '',
        },
        patronymic: {
          name: 'patronymic',
          title: 'Отчество',
          value: '',
        },
        birthdate: {
          name: 'birthdate',
          title: 'День рождения',
          error: false,
          errorMessage: '',
          value: '',
        },
        phone: {
          name: 'phone',
          title: 'Номер телефона',
          error: false,
          errorMessage: '',
          value: '',
        },
        sex: {
          name: 'sex',
          title: 'Пол',
          value: '',
        },
        clientsGroup: {
          name: 'clientsGroup',
          title: 'Группа клиентов',
          error: false,
          errorMessage: '',
          value: '',
          options: ['VIP', 'Проблемные', 'ОМС'],

        },
        selectedDoctor: {
          name: 'selectedDoctor',
          title: 'Лечащий врач',
          value: '',
          options: ['Иванов', 'Захаров', 'Чернышева'],
          error: false,
          errorMessage: '',
        },
        subscribeSms: {
          name: 'subscribeSms',
          title: 'Не отправлять СМС',
          value: false,
          error: false,
          errorMessage: '',
        },
        index: {
          name: 'index',
          title: 'Индекс',
          value: '',
          error: false,
          errorMessage: '',
        },
        country: {
          name: 'country',
          title: 'Страна',
          value: '',
          error: false,
          errorMessage: '',
        },
        region: {
          name: 'region',
          title: 'Регион',
          value: '',
          error: false,
          errorMessage: '',
        },
        city: {
          name: 'city',
          title: 'Город',
          error: false,
          errorMessage: '',
          value: '',
        },
        street: {
          name: 'street',
          title: 'Улица',
          value: '',
          error: false,
          errorMessage: '',
        },
        house: {
          name: 'house',
          title: 'Дом',
          value: '',
          error: false,
          errorMessage: '',
        },
        doctype: {
          name: 'doctype',
          title: 'Тип документа',
          error: false,
          errorMessage: '',
          value: '',
          options: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
        },
        docSerie: {
          name: 'docSerie',
          title: 'Серия',
          value: '',
          error: false,
          errorMessage: '',
        },
        docNum: {
          name: 'docNum',
          title: 'Номер',
          value: '',
          error: false,
          errorMessage: '',
        },
        docBy: {
          name: 'docBy',
          title: 'Кем выдан',
          value: '',
          error: false,
          errorMessage: '',
        },
        docDate: {
          name: 'docDate',
          title: 'Дата выдачи',
          value: '',
          error: false,
          errorMessage: '',
        },
      },
    };
  },
  validations: {
    fields: {
      secondName: {
        value: {
          required,
          minLength: minLength(2),
        },
      },
      firstName: {
        value: {
          required,
        },
      },
      birthdate: {
        value: {
          required,
        },
      },
      clientsGroup: {
        value: {
          required,
        },
      },
      docDate: {
        value: {
          required,
        },
      },
      doctype: {
        value: {
          required,
        },
      },
      city: {
        value: {
          required,
        },
      },
      phone: {
        value: {
          required,
        },
      },
    },

  },

  methods: {
    setErrorMessage(errorField) {
      let message = '';

      if (this.$v.fields[errorField].value.required === false) {
        message = errorMessages.required;
      }
      if (this.$v.fields[errorField].value.minLength === false) {
        message = errorMessages.minLength;
      }
      this.fields[errorField].errorMessage = message;
      console.log('Error', message, ' in ', this.fields[errorField].name);
    },
    changeCheckBoxData(args) {
      const { name, val } = args;
      if (val) {
        this.fields[name].error = false;
        this.fields[name].errorMessage = '';
      }
    },
    changeFieldData(args) {
      const { name, value } = args;
      if (value.length) {
        this.fields[name].error = false;
        this.fields[name].errorMessage = '';
      }

      this.$v.fields[name].value.$touch();
      console.log('Submit!', this.$v.fields[name].value);
      if (this.$v.fields[name].value.$invalid) {
        this.fields[name].error = true;
        this.setErrorMessage(name); s;
      }
    },
    processForm() {
      this.$v.$touch();

      const allValidatedFileds = Object.keys(this.$v.fields.$model);

      const invalidFields = Object.keys(this.$v.fields).filter(
        (fieldName) => this.$v.fields[fieldName].$invalid,
      );

      allValidatedFileds.forEach((field) => {
        if (!invalidFields.includes(field)) {
          this.fields[field].error = false;
          this.fields[field].errorMessage = '';
        }
      });

      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR';

        Object.keys(this.fields).forEach((field) => {
          invalidFields.filter((invalid) => {
            if (invalid === this.fields[field].name) {
              Object.keys(this.$v.fields).filter((vField) => {
                if (vField === this.fields[field].name) {
                  this.setErrorMessage(vField);
                }
                return true;
              });

              this.fields[field].error = true;
              return true;
            }
            return true;
          });
        });
        console.log('ERROR');
      } else {
        Object.keys(this.fields).forEach((field) => {
          console.log('filed', field);
          this.fields[field].error = false;
          this.fields[field].errorMessage = '';
        });
        console.log('PENDING');
        this.submitStatus = 'PENDING';
        setTimeout(() => {
          this.submitStatus = 'OK';
          console.log('OK');
        }, 500);
      }
    },
  },
};
</script>

<style>
.form {
  width: 400px;
  margin: 0 auto;
}
</style>
