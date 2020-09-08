<template>
<div
  class="dropdown__label"
  v-bind:class="{opened: isOpened, error: item.error}"
  data-prop="dropdown"
  v-on:click="modifiedOn"
  data-element="">
  <div
    class="dropdown__placeholder "
    v-bind:class="{modified: isModified}"
    data-prop="placeholder"
    ><span v-if="item.required">*</span>{{item.title}}</div>
  <div class="input__err-block">{{item.errorMessage}}</div>
  <div class="dropdown__field" data-prop="field">{{value}}</div>
  <input type="hidden" v-bind:name="item.name" data-prop="select" id="field_role" value="" />
  <ul class="dropdown__menu" v-bind:class="{opened: isOpened}">
    <li
    v-for="opt in item.options"
    v-bind:key="opt"
    class="dropdown__menu-item"
    data-prop="option"
    value="opt" v-on:click="choseOption(item.name, opt)">{{opt}}</li>
  </ul>
</div>
</template>

<script>
// import Vue from 'vue'

export default {

  name: 'Select',
  data() {
    return {
      isModified: false,
      isOpened: false,
      value: '',
    };
  },
  props: {
    item: {
      type: Object,
      // required: true
    },
  },
  created() {
    window.addEventListener('click', this.modifiedOff);
  },

  beforeDestroy() {
    window.removeEventListener('click', this.modifiedOff);
  },

  methods: {
    modifiedOn() {
      this.isModified = true;
      this.isOpened = true;
    },
    modifiedOff(e) {
      if (!this.$el.contains(e.target)) {
        this.isOpened = false;
        this.isModified = this.value.length > 0;
      }
    },
    choseOption(name, val) {
      this.isModified = true;
      this.isOpened = false;
      this.value = val;
      this.$emit('input', val);
      this.$emit('changedItemData', { name, val });
    },
  },

};
</script>

<style>
.dropdown__label[data-prop=dropdown] {
  cursor: pointer;
}

.dropdown__label {
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

.dropdown__placeholder {
  font-size: 16px;
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

.dropdown__placeholder.modified {
  font-size: 12px;
  top: 10px;
  left: 12px;
}

.dropdown__placeholder span {
  color: #f23030;
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

.dropdown__label.error .input__err-block {
  display: block;
}

.dropdown__field {
  font-size: 16px;
  line-height: normal;
  height: 56px;
  width: 100%;
  padding: 25px 0 10px 12px;
  border: none;
  position: absolute;
  left: 0;
  bottom: 0;
  text-align: left;
}

.dropdown__menu {
  width: 100%;
  max-height: 180px;
  overflow-y: auto;
  list-style: none;
  border: solid 1px #cccccc;
  background: #ffffff;
  display: none;
  position: absolute;
  top: 56px;
  left: -1px;
  z-index: 200;
  padding: 0;
  margin: 0;

}

.dropdown__menu.opened {
  display: block;
}

.dropdown__label[data-prop=dropdown]::after {
  content: '';
  width: 10px;
  height: 10px;
  position: absolute;
  top: 20px;
  right: 20px;
  border-left: 2px solid #000;
  border-bottom: 2px solid #000;
  transform: rotate(-45deg);
}

.dropdown__label[data-prop=dropdown].opened::after {
  transform: rotate(135deg);
}

.dropdown__menu-item {
  font-size: 16px;
  width: 100%;
  padding: 12px;
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  -moz-transition: all 0.3s ease;
  transition: all 0.3s ease;
}

.dropdown__menu-item.picked {
  font-weight: bold;
}

.dropdown__label.error {
  border: solid 1px #ff7676;
}

.dropdown__label.error::before {
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
