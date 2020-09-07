<template>
<form class="form" @submit.prevent="processForm">
  <template v-for="item in fields">
    <Select v-if="item.options" v-bind:key="item.name" v-bind:item="item" v-model="item.value" />
    <Checkbox
    v-else-if="typeof item.value === 'boolean'"
    :key="item.name"
    v-bind:item="item"
    v-model="item.value" />
    <TextInput v-else v-bind:key="item.name" v-bind:item="item" v-model="item.value" />
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
    processForm() {
      console.log('Submit!', this.$v);
      this.$v.$touch();
      // если есть хоть какие-то ошибки
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR';
        const invalidFields = Object.keys(this.$v.fields).filter(
          (fieldName) => this.$v.fields[fieldName].$invalid,
        );
        console.log('invalidFields', invalidFields);
        Object.keys(this.fields).forEach((field) => {
          // console.log('field', this.fields[field].name);
          invalidFields.filter((invalid) => {
            if (invalid === this.fields[field].name) {
              // console.log('hohohoho', this.fields[field].name);
              // // TODO: сюда включить поиск какая именно ошибка у этого поля и
              // // добавить errorMessage
              Object.keys(this.$v.fields).filter((vField) => {
                if (vField === this.fields[field].name) {
                  let message = '';

                  if (this.$v.fields[vField].value.required === false) {
                    message = errorMessages.required;
                  }
                  if (this.$v.fields[vField].value.minLength === false) {
                    message = errorMessages.minLength;
                  }
                  this.fields[vField].errorMessage = message;
                  console.log('Error', message, ' in ', this.fields[field].name);
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
        // this.fields.forEach(field => {
        //   field.error = false;
        //   field.errorMessage = ''
        // })
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
