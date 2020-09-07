<template>
<label class="input__label" v-bind:class="{error: item.error}">
  <div class="input__placeholder " v-bind:class="{modified: isModified}" data-prop="placeholder">
    <span>*</span>{{item.title}}</div>
  <div class="input__err-block">{{item.errorMessage}}</div>
  <input
  class="input__input"
  type="text"
  :name="item.name"
  v-on:click="setModified($event.target.value)"
  v-on:focus="setModified($event.target.value)"
  v-on:change="setModified($event.target.value)"
  v-on:blur="setModified($event.target.value)"
  v-on:input="updateValue($event)"
   />
</label>
</template>

<script>
export default {
  name: 'TextInput',
  props: {
    item: {
      type: Object,
    },
  },
  data() {
    return {
      isModified: false,
    };
  },
  methods: {
    setModified(value) {
      this.isModified = !!value.length;
    },
    updateValue(e) {
      //  this.$emit('updateFieldValue', [e.target.name, e.target.value]);
      // console.log('event', e.target.name);
      this.setModified(e.target.value);
      this.$emit('input', e.target.value);
      this.$emit('changedItemData', { name: e.target.name, value: e.target.value });
    },
  },

};
</script>

<style>
.input__label {
  line-height: normal;
  height: 58px;
  width: 100%;
  margin-bottom: 20px;
  border: solid 1px #cccccc;
  cursor: text;
  display: block;
  position: relative;
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  -moz-transition: all 0.3s ease;
  transition: all 0.3s ease;
}

.input__placeholder {
  font-size: 16px;
  font-weight: normal;
  font-style: italic;
  color: #b3b3b3;
  position: absolute;
  left: 12px;
  top: 16px;
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  -moz-transition: all 0.3s ease;
  transition: all 0.3s ease;
  z-index: 150;
}

.input__err-block {
  font-size: 12px;
  font-style: italic;
  color: #ff7676;
  position: absolute;
  right: 45px;
  top: 20px;
  z-index: 150;
  display: none;
}

.input__label.error .input__err-block {
  display: block;
}

input[type="text"].input__input {
  font-size: 16px;
  width: 100%;
  height: 58px;
  padding: 25px 0 10px 12px;
  border: none;
  box-sizing: border-box;
  position: absolute;
  left: 0;
  bottom: 0;
}

input[type="text"].input__input:focus {
  outline: none;
  border: none;
}

.input__placeholder.modified {
  font-size: 12px;
  top: 10px;
  left: 12px;
}

.input__placeholder span {
  color: #f23030;
}

.input__label.error {
  border: solid 1px #ff7676;
}

.input__label.error::before {
  content: '';
  height: 58px;
  width: 3px;
  background: #ff7676;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 100;
}
</style>
