<template>
<div id="myModal" class="modal" v-bind:class="{hidden}">
  <div class="modal-content">
    <span class="close" @click="closeModal()">&times;</span>
    <p>{{modalStatus}}</p>
  </div>

</div>
</template>

<script>
export default {
  data() {
    return {

    };
  },
  props: {
    hidden: Boolean,
    modalStatus: String,
  },
  methods: {
    closeModal() {
      this.$emit('hideModal', true);
    },
  },

};
</script>

<style>
.modal.hidden {
  display: none;
}
.modal {
  position: fixed;
  z-index: 200;
  padding-top: 100px;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0,0,0);
  background-color: rgba(0,0,0,0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
</style>
