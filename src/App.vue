<template>
  <div id="app">
    <md-whiteframe>
      <md-toolbar>
        <span class="md-title">Password Generator</span>
      </md-toolbar>
    </md-whiteframe>

    <div class="page-layout">
      <div class="container">
        <md-input-container>
          <label>Password</label>
          <md-input type="text" v-model="password" ></md-input>
        </md-input-container>
        <md-input-container>
          <label>Password Length</label>
          <md-input type="number" v-model="passwordLength"></md-input>
        </md-input-container>
        <div class="switch-container">
          <md-switch class="md-primary" v-model="includeSymbols">Include Symbols, e.g. @#$%</md-switch>
          <md-switch class="md-primary" v-model="includeNumbers">Include Numbers, e.g. 123456</md-switch>
          <md-switch class="md-primary" v-model="includeLowercaseCharacters">Include Lowercase Characters, e.g. abcdefgh</md-switch>
          <md-switch class="md-primary" v-model="includeUppercaseCharacters">Include Uppercase Characters, e.g. ABCDEFGH</md-switch>
        </div>
        <div class="button-container">
          <md-button class="md-raised md-primary" @click="generatePassword">Generate Again</md-button>
          <md-button class="md-raised" v-clipboard:copy="password"  v-clipboard:success="onCopy"><md-icon>content_copy</md-icon> Copy</md-button>
        </div>
        <md-snackbar :md-position="vertical + ' ' + horizontal" ref="snackbar" :md-duration="duration">
        <span>Copied!</span>
        </md-snackbar>
      </div>
    </div>
    <div class="page-layout">
    <qrcode :value="password" :options="{ size: 200 }"></qrcode>
    </div>
    <div class="footer">
      <!-- Place this tag where you want the button to render. -->
      <div>
        <span class="md-caption">Client-size password generator.</span><br />
        <span class="md-caption">Donations are always appreciated! <br />
          ETH: 0x019463d7ACB16f9fD8ad17537d413Ef061e8d043</span>
      <br />
      <br />
      <!-- Place this tag where you want the button to render. -->
      <a class="github-button" href="https://github.com/thanhson1085/pwgen" data-icon="octicon-star" aria-label="Star thanhson1085/pwgen on GitHub">Star</a>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.css'
import VueClipboard from 'vue-clipboard2'
import VueQrcode from '@xkeshi/vue-qrcode'

Vue.use(VueMaterial)
Vue.use(VueClipboard)
Vue.component('qrcode', VueQrcode)

Vue.material.registerTheme('blue', {
  primary: 'blue',
  accent: 'red',
  warn: 'red'
})

Vue.material.setCurrentTheme('blue')

export default {
  name: 'app',
  data() {
    return {
      includeSymbols: false,
      includeNumbers: true,
      includeLowercaseCharacters: true,
      includeUppercaseCharacters: true,
      passwordLength: 30,
      password: '',
      vertical: 'bottom',
      horizontal: 'center',
      duration: 4000
    };
  },
  watch: {
    '$route'() {
      this.$nextTick(() => {
        window.Prism.highlightAll();
        this.$refs.pageContent.scrollTop = 0;
        this.showSidebar = false;
      });
    }
  },
  created() {
    this.generatePassword();
  },
  mounted() { },
  methods: {
    onCopy() {
      this.$refs.snackbar.open()
    },
    generatePassword() {
      const lowercaseCharacters = 'qwertyuiopasdfghjklzxcvbnm';
      const uppercaseCharacters = 'QWERTYUIOPASDFGHJKLZXCVBNM';
      const symbols = '@#$%[]{}!^';
      const numbers = '1234567890';
      const randoms = [];
      if (this.includeSymbols) {
        randoms.push(symbols);
      }
      if (this.includeNumbers) {
        randoms.push(numbers);
      }
      if (this.includeLowercaseCharacters) {
        randoms.push(lowercaseCharacters);
      }
      if (this.includeUppercaseCharacters) {
        randoms.push(uppercaseCharacters);
      }
      this.password = '';
      for (var i=0; i < this.passwordLength; i++) {
        const a = randoms[Math.floor(Math.random() * randoms.length)];
        const c = a.charAt(Math.floor(Math.random() * a.length));
        this.password += c;
      }
    }
  }
};
</script>
<style>
  .page-layout, .footer {
    padding-top: 30px;
    margin: 16px auto;
    display: flex;
    max-width: 600px;
  }
  .contaner {
    width: 100%;
  }
  .switch-container .md-switch {
    width: 100%;
  }
  .button-container {
    padding-top: 20px;
  }
</style>
