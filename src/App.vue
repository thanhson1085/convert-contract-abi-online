<template>
  <div id="app">
    <md-whiteframe>
      <md-toolbar md-theme="blue">
        <span class="md-title">Ethereum Contract ABI Converter</span>
      </md-toolbar>
    </md-whiteframe>

    <div class="page-layout">
      <form novalidate @submit.prevent="">
        <div class="field-group">
          <md-layout md-gutter v-for="p in components">
            <md-layout md-flex="30">
              <md-input-container>
              <label for="country">Parameter Type</label>
              <md-select name="parameterType" id="parameterType" v-model="p.type">
              <md-option v-for="(value, key) in parameterTypes" :value="key" :key="key">{{value}}</md-option>
              </md-select>
              </md-input-container>
            </md-layout>
            <md-layout>
              <md-input-container>
                <label>Parameter Value</label>
                <md-input v-model="p.value" placeholder="Parameter Value"></md-input>
              </md-input-container>
            </md-layout>
          </md-layout>
          <md-button class="md-raised md-primary" @click="addComponent()"><md-icon>add</md-icon> Add Parameter</md-button>
          <md-button class="md-raised md-accent" @click="generateAbi()">Generate ABI</md-button>
          <md-input-container>
          <label>ABI-Encoded Output</label>
          <md-textarea id="output" v-model="encoded"></md-textarea>
          </md-input-container>
          <md-button class="md-raised" v-clipboard:copy="encoded"  v-clipboard:success="onCopy"><md-icon>content_copy</md-icon> {{copy}}</md-button>
        </div>
      </form>
    </div>
    <div class="footer">
      <!-- Place this tag where you want the button to render. -->
      <div>
        <br />
        <span class="md-caption">Donations are always appreciated! <br />
          ETH: 0x019463d7ACB16f9fD8ad17537d413Ef061e8d043</span>
      <br />
      <br />
      <!-- Place this tag where you want the button to render. -->
      <a class="github-button" href="https://github.com/thanhson1085/covert-contract-abi-online" data-icon="octicon-star" aria-label="Star thanhson1085/covert-contract-abi-online on GitHub">Star</a>
    </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.css'
import VueClipboard from 'vue-clipboard2'

import abi from 'ethereumjs-abi'


Vue.use(VueMaterial)
Vue.use(VueClipboard)
var parameterTypes = {
  'string': 'String',
  'address': 'Address',
  'address[]': 'Address[]',
  'uint': 'Uint',
  'uint256': 'Uint256'
}

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      encoded: '',
      parameter: '',
      components: [{type: 'string', value: ''}],
      parameterType: 'string',
      parameterTypes: parameterTypes,
      copy: 'Copy'
    }
  },
  methods: {
    addComponent() {
      this.components.push({type: 'string', value: ''})
    },
    onCopy() {
      this.copy = 'Copied'
      setTimeout(() => {
        this.copy = 'Copy'
      }, 3000)
    },
    generateAbi() {
      var types = this.components.filter(c => c.value).map(c => c.type)
      var values = this.components.map(c => c.value).filter(v => v)
      this.encoded = abi.rawEncode(types, values).toString('hex')
    }
  }
}
</script>

<style>
  .page-layout, .footer {
    padding-top: 30px;
    display: flex;
    margin: 16px auto;
    max-width: 600px;
  }
  form {
    width: 100%;
  }
  #output {
    min-height: 80px;
  }
</style>
