<template>
  <div id="app">
    <div class="global-wrapper flex-container">
      <Iphone></Iphone>
      <div class="container">
        <div class="form-group mt-5">
          <label for="txtBox">
            <h5>PinPad</h5>
          </label>
          <input
            v-show="!message"
            type="password"
            class="form-control"
            v-model="value"
            id="txtBox"
            placeholder=""
            disabled="disabled"
          />
          <input
            v-show="message"
            type="text"
            class="form-control"
            v-model="message"
            id="msgBox"
            placeholder=""
            disabled="disabled"
          />
        </div>
        <Keyboard @pressed="value = $event" :selfValue="value" :trueCode="1942" @displayMessage="message = $event"></Keyboard>
      </div>
    </div>
  </div>
</template>

<script>
import Iphone from './components/Iphone.vue';
import Keyboard from './components/Keyboard.vue';

export default {
  components: { Keyboard, Iphone },
  data() {
    return {
      value: '',
      message: '',
    };
  },
};
</script>

<style scoped>
.form-group label h5 {
  color: white;
}
.container {
  position: absolute;
  max-width: 300px;
}
.global-wrapper {
  position: relative;
  margin-top: 400px;
}
.flex-container {
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-direction: row;
  -ms-flex-direction: row;
  flex-direction: row;
  -webkit-flex-wrap: nowrap;
  -ms-flex-wrap: nowrap;
  flex-wrap: nowrap;
  -webkit-justify-content: center;
  -ms-flex-pack: center;
  justify-content: center;
  -webkit-align-content: center;
  -ms-flex-line-pack: center;
  align-content: center;
  -webkit-align-items: center;
  -ms-flex-align: center;
  align-items: center;
  }
</style>
