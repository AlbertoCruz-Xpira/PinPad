<template>
  <div class="numeric-keyboard rounded border p-3">
    <button
      v-for="key in keys"
      :key="key"
      @click="press(key)"
      class="btn btn-lg shadow-none"
      :class="keyTheme"
    >{{ key }}</button>
    <button class="btn btn-lg shadow-none" :class="buttonTheme" @click="clear()">&larr;</button>
    <button class="btn btn-lg shadow-none" :class="buttonTheme" @click="clear('all')">C</button>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css';
import _ from 'lodash';

export default {
  props: ['selfValue', 'trueCode'],
  data() {
    return {
      value: '',
      message: '',
      errors: 0,
      locked: false,
      keys: [...Array(10).keys()],
      keyTheme: 'btn-keyboard',
      buttonTheme: 'btn-keyboard',
    };
  },
  methods: {
    shuffle() {
      this.keys = _.shuffle(this.keys);
    },
    press(key) {
      if (this.locked == false) {
        this.clearMsg();
        this.value = `${this.value}${key}`;
        this.shuffle();
        if (this.value.length >= 4) {
          if (this.value == this.trueCode) {
            this.message = 'OK';
            this.errors = 0;
            this.clear();
          } else {
            this.message = 'ERROR';
            this.errors += 1;
            this.clear();
            if (this.errors === 3) {
              this.message = 'LOCKED';
              this.errors = 0;
              this.clear();
              this.locked = true;
              setTimeout(() => {
                this.locked = false;
                this.clearMSG;
              }, 20000);
            }
          }
          this.value = '';
        }
      }
    },
    clear(type) {
      if (type === 'all') this.value = '';
      else this.value = this.value.substring(0, this.value.length - 1);
    },
    clearMsg() {
      this.message = '';
    }
  },
  watch: {
    value() {
      this.$emit('pressed', this.value);
    },
    selfValue() {
      this.value = this.selfValue;
    },
    message() {
      this.$emit('displayMessage', this.message);
    }
  },
  created() {
    this.shuffle();
  },
};
</script>

<style scoped>
.numeric-keyboard {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-gap: 10px;
  color: #3D0F2B;
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 25px;
  background-color: #f0daf0 0%;
  background-image: -moz-linear-gradient(#f0daf0 0%, #ffd582 100%);
  background-image: -webkit-gradient(linear, 0% 0%, 0% 100%, from(#f0daf0 0%), to(#ffd582 100%));
  background-image: -webkit-linear-gradient(#f0daf0 0%, #ffd582 100%);
  background-image: -o-linear-gradient(#f0daf0 0%, #ffd582 100%);
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f0daf0', endColorstr='#ffd582', GradientType=1 );
}
.numeric-keyboard .btn {
  font-weight: bold;
  color: #3b305d;
}
.btn-keyboard {
  background: none !important;
}
.btn-keyboard:hover {
  background-color: #ff980087 !important;
}
.form-control:disabled, .form-control[readonly] {
  font-size: 25px;
  font-weight: 400;
  background-color: #dae6f1;
}
.btn-keyboard {
  background-color: #80b6ff;
  color: #ffffff;
}

</style>
