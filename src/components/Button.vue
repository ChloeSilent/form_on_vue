<template>
<button class="btn btn--round" type="submit">Отправить</button>
</template>

<script>
export default {
  name: 'Button',
  methods: {
    // setSubmit: function () {
    //   this.$emit('submit-form');
    // }
  },
};
</script>

<style>
.btn {
  font-size: 14px;
  font-weight: normal;
  text-align: center;
  text-decoration: none;
  letter-spacing: 0.3px;
  color: #000000;
  line-height: 1.42857143;
  white-space: nowrap;
  min-width: 170px;
  padding: 6px 12px;
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
  border: solid 1px #4257b2;
  background: #cccccc;
  cursor: pointer;
  position: relative;
  display: inline-block;
  vertical-align: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  -moz-transition: all 0.3s ease;
  transition: all 0.3s ease;
}

.btn.btn--round {
  font-weight: bold;
  -webkit-border-radius: 20px;
  -moz-border-radius: 20px;
  border-radius: 20px;
}

.btn:hover {
  color: #cccccc;
  outline: none;
  border: solid 1px #cccccc;
  background: #4257b2;
}

.btn:focus {
  color: #ffffff;
  outline: none;
  -webkit-box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  -moz-box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}
</style>
