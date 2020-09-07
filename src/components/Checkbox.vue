<template>
<label class="checkbox" v-bind:class="{error: item.error}">
  <input class="checkbox__input"
  id="field_terms"
  type="checkbox"
  name="item.name"
  v-bind:checked="item.value"
  v-on:change="updateValue($event)">
  <div class="checkbox__err">{{item.errorMessage}}</div>
  <span class="checkbox__text">{{item.title}}</span>
  <span class="checkbox__checkmark"></span>
</label>
</template>

<script>
export default {
  name: 'Checkbox',
  props: {
    item: {
      type: Object,
    },
  },
  methods: {
    updateValue(event) {
      console.log(event.target.checked);
      this.$emit('input', event.target.checked);
      const { name } = event.target;
      const val = event.target.checked;
      this.$emit('changeCheckBoxData', { name, val });
    },
  },
};
</script>

<style>
.checkbox {
  font-weight: normal;
  margin: 20px 0 40px 0;
  cursor: pointer;
  display: inline-block;
  position: relative;
}

.checkbox__input {
  height: 0;
  width: 0;
  opacity: 0;
  position: absolute;
}

input[type="checkbox"] {
  margin: 4px 0 0;
  margin-top: 1px \9;
  line-height: normal;
  box-sizing: border-box;
  padding: 0;
}

.checkbox {
  font-weight: normal;
  margin: 20px 0 40px 0;
  cursor: pointer;
  display: inline-block;
  position: relative;
}

.checkbox__text {
  font-size: 16px;
  padding-left: 25px;
}

.checkbox.error .checkbox__text {
  color: #f23030;
}

.checkbox__checkmark {
  width: 16px;
  height: 16px;
  border: solid 1px #cccccc;
  background: #ffffff;
  position: absolute;
  top: 0;
  left: 0;
}

.checkbox.error .checkbox__checkmark {
  border-color: #f23030;
}

.checkbox input:checked~.checkbox__checkmark {
  border: solid 1px #cccccc;
  background: #cccccc;
}

.checkbox__checkmark::after {
  content: '';
  width: 5px;
  height: 10px;
  display: none;
  border: solid #000000;
  border-width: 0 3px 3px 0;
  position: absolute;
  top: 0;
  left: 4px;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

.checkbox input:checked~.checkbox__checkmark:after {
  display: block;
}
</style>
