<template>
<div>
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
    v-on:changedItemData="changeFieldData($event)"
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
<Modal
v-on:hideModal="changeModalVisibility($event)"
v-bind:hidden="modalHidden"
v-bind:modalStatus="status"/>
</div>
</template>

<script>
import {
  required, minLength, maxLength, numeric,
} from 'vuelidate/lib/validators';

import TextInput from './TextInput.vue';
import Select from './Select.vue';
import Checkbox from './Checkbox.vue';
import Button from './Button.vue';
import Modal from './Modal.vue';

const errorMessages = {
  required: 'Заполните поле',
  minLength: 'Hедостаточно символов',
  maxLength: 'Не больше чем 11 символов',
  numeric: 'Введено не число',
  correctPhone: 'Длина 11 цифр. Начинается с 7',
};

export default {
  name: 'Form',
  components: {
    TextInput,
    Select,
    Checkbox,
    Button,
    Modal,
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
          type: 'text',
        },
        firstName: {
          name: 'firstName',
          title: 'Имя',
          error: false,
          errorMessage: '',
          value: '',
          type: 'text',
        },
        patronymic: {
          name: 'patronymic',
          title: 'Отчество',
          error: false,
          errorMessage: '',
          value: '',
          type: 'text',
        },
        birthdate: {
          name: 'birthdate',
          title: 'День рождения',
          error: false,
          errorMessage: '',
          value: '',
          type: 'date',
        },
        phone: {
          name: 'phone',
          title: 'Номер телефона',
          error: false,
          errorMessage: '',
          value: '',
          type: 'text',
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
          error: false,
          errorMessage: '',
          value: '',
          options: ['Иванов', 'Захаров', 'Чернышева'],
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
          type: 'text',
        },
        country: {
          name: 'country',
          title: 'Страна',
          value: '',
          error: false,
          errorMessage: '',
          type: 'text',
        },
        region: {
          name: 'region',
          title: 'Регион',
          value: '',
          error: false,
          errorMessage: '',
          type: 'text',
        },
        city: {
          name: 'city',
          title: 'Город',
          error: false,
          errorMessage: '',
          value: '',
          type: 'text',
        },
        street: {
          name: 'street',
          title: 'Улица',
          value: '',
          error: false,
          errorMessage: '',
          type: 'text',
        },
        house: {
          name: 'house',
          title: 'Дом',
          value: '',
          error: false,
          errorMessage: '',
          type: 'text',
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
          type: 'text',
        },
        docNum: {
          name: 'docNum',
          title: 'Номер',
          value: '',
          error: false,
          errorMessage: '',
          type: 'text',
        },
        docBy: {
          name: 'docBy',
          title: 'Кем выдан',
          value: '',
          error: false,
          errorMessage: '',
          type: 'text',
        },
        docDate: {
          name: 'docDate',
          title: 'Дата выдачи',
          value: '',
          error: false,
          errorMessage: '',
          type: 'date',
        },
      },
      modalHidden: true,
      status: '',
    };
  },
  validations: {
    fields: {
      secondName: {
        value: {
          required,
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
          minLength: minLength(11),
          maxLength: maxLength(11),
          numeric,
          correctPhone(phoneNumber) {
            return /^7\d{10}/.test(phoneNumber);
          },
        },
      },
    },

  },

  methods: {
    changeModalVisibility(args) {
      console.log('modal', args);
      this.modalHidden = true;
    },
    setErrorMessage(errorField) {
      let message = '';

      if (this.$v.fields[errorField].value.required === false) {
        message = errorMessages.required;
      }
      if (this.$v.fields[errorField].value.minLength === false) {
        message = errorMessages.minLength;
      }
      if (this.$v.fields[errorField].value.maxLength === false) {
        message = errorMessages.maxLength;
      }
      if (this.$v.fields[errorField].value.correctPhone === false) {
        message = errorMessages.correctPhone;
      }
      this.fields[errorField].errorMessage = message;
    },
    changeFieldData(args) {
      const { name, val } = args;
      if (val) {
        this.fields[name].error = false;
        this.fields[name].errorMessage = '';
      }
      if (this.$v.fields[name]) {
        this.$v.fields[name].value.$touch();

        if (this.$v.fields[name].value.$invalid) {
          this.fields[name].error = true;
          this.setErrorMessage(name);
        }
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
        this.modalHidden = false;
        this.status = 'Исправьте ошибки';
      } else {
        Object.keys(this.fields).forEach((field) => {
          this.fields[field].error = false;
          this.fields[field].errorMessage = '';
        });
        console.log('PENDING');
        this.submitStatus = 'PENDING';
        setTimeout(() => {
          this.submitStatus = 'OK';
          console.log('OK');
          this.modalHidden = false;
          this.status = 'Ваша заявка принята';
        }, 500);
      }
    },
  },
};
</script>

<style>
.form {
  min-width: 300px;
  max-width: 1024px;
  width: 100%;
  margin: 0 auto;
}
</style>
