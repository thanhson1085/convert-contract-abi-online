<template>
  <div id="app">
    <md-whiteframe>
      <md-toolbar md-theme="blue">
        <span class="md-title">Convert Ethereum Contract ABI Online</span>
      </md-toolbar>
    </md-whiteframe>

    <div class="page-layout">
      <form novalidate @submit.stop.prevent="submit">
        <md-input-container>
          <label>Contract Parameters</label>
          <md-input v-model="parameters" placeholder="Type your contract parameters"></md-input>
        </md-input-container>
        <md-input-container>
        <label>Textarea</label>
        <md-textarea maxlength="70" v-model="encoded"></md-textarea>
        </md-input-container>
      </form>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.css'

import abi from 'ethereumjs-abi'

var parameterTypes = [
    'string',
    'string',
    'uint256',
    'string'
]
var parameterValues = [
    'Test Token',
    'TST',
    '18',
    '1.0'
]

var encoded = abi.rawEncode(parameterTypes, parameterValues)

Vue.use(VueMaterial)

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      encoded: encoded.toString('hex'),
      parameters: ''
    }
  }
}
</script>

<style>
  .page-layout {
    margin: 16px 15%;
    display: flex;
  }
  form {
    width: 100%;
  }
</style>
