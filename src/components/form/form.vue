<template>
  <form class="t-form">
    <slot></slot>
  </form>
</template>

<script>
import ValidateHelper from '../../mixins/validateHelper'
import AsyncValidator from 'async-validator'
import Emitter from '../../mixins/emitter'

//  TODO: new interface for custom error message by own
//  TODO: validateIcon
//  TODO: rebuild validate trigger

export default {
  name: 't-form',

  mixins: [ValidateHelper, Emitter],

  provide: {
    TForm: this
  },

  data () {
    return {
      model: {}
    }
  },

  props: {
    rules: {},
    layout: String
  },

  mounted () {
    this.rules && this.broadcast('t-form-item', 'set-form-item-rule', this.rules)
    this.layout && this.broadcast('t-form-item', 'set-form-item-layout', this.layout)
    this.$on('form-set-model', this.setModel)
    this.$on('form-validate', this.validate)
    this.$on('form-reset', this.reset)
    this.$on('form-get-validator', this.validator)
  },

  methods: {
    setModel ({prop, value}) {
      this.model[prop] = value
    },
    validator () {
      this.broadcastAllInput('submit')
      return this.rules ? new AsyncValidator(this.rules) : false
    },
    reset () {
      this.broadcastAllInput('reset')
    },
    broadcastAllInput (type) {
      this.broadcast('t-input', type)
      this.broadcast('t-select', type)
      this.broadcast('t-cascader', type)
      this.broadcast('t-checkbox', type)
      this.broadcast('t-checkbox-group', type)
      this.broadcast('t-radio', type)
      this.broadcast('t-radio-group', type)
      this.broadcast('t-date-picker', type)
      this.broadcast('t-time-picker', type)
    }
  },
  watch: {
    layout (val) {
      this.broadcast('t-form-item', 'set-form-item-layout', val)
    }
  }
}
</script>

<style scoped>

</style>
