<script lang="ts">
/**
 * vue-toggles
 * @author Julian <https://github.com/juliandreas>
 * @license MIT
 * @link https://github.com/juliandreas/vue-toggles
 * Modified by marverix to work with Vue.js 3 + TypeScript
 */

import { defineComponent } from 'vue';

export default defineComponent({

  name: 'VueToggles',

  props: {
    modelValue: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    reverse: {
      type: Boolean,
      default: false,
    },
    checkedText: {
      type: String,
      default: null,
    },
    uncheckedText: {
      type: String,
      default: null,
    },
    width: {
      type: Number,
      default: 75,
    },
    height: {
      type: Number,
      default: 25,
    },
    uncheckedBg: {
      type: String,
      default: '#939393',
    },
    checkedBg: {
      type: String,
      default: '#5850ec',
    },
    dotColor: {
      type: String,
      default: '#fff',
    },
    fontSize: {
      type: String,
      default: '12',
    },
    checkedColor: {
      type: String,
      default: '#fff',
    },
    uncheckedColor: {
      type: String,
      default: '#fff',
    },
    fontWeight: {
      type: String,
      default: 'normal',
    },
  },

  emits: ['update:modelValue'],

  computed: {

    value: {
      get() {
        return this.modelValue;
      },

      set(value: boolean) {
        this.$emit('update:modelValue', value)
      }
    },

    bgStyle() {
      const styles = {
        width: `${this.width}px`,
        height: `${this.height}px`,
        background: this.value ? this.checkedBg : this.uncheckedBg,
        opacity: this.disabled ? '0.5' : '1',
        cursor: !this.disabled ? 'pointer' : 'not-allowed',
      };

      return styles;
    },

    dotStyle() {
      const styles = {
        background: this.dotColor,
        width: `${this.height - 8}px`,
        height: `${this.height - 8}px`,
        'min-width': `${this.height - 8}px`,
        'min-height': `${this.height - 8}px`,
        'margin-left': this.value ? `${this.width - (this.height - 3)}px` : '5px',
      };

      if ((!this.value && this.reverse) || (this.value && !this.reverse)) {
        styles['margin-left'] = `${this.width - (this.height - 3)}px`;
      } else if ((this.value && this.reverse) || (!this.value && !this.reverse)) {
        styles['margin-left'] = '5px';
      }

      return styles;
    },

    textStyle() {
      const styles = {
        'font-weight': this.fontWeight,
        'font-size': `${this.fontSize}px`,
        color: this.value && !this.disabled ? this.checkedColor : this.uncheckedColor,
        right: this.value ? `${this.height - 3}px` : 'auto',
        left: this.value ? 'auto' : `${this.height - 3}px`,
      };

      if (!this.value && this.reverse) {
        styles.right = `${this.height - 3}px`;
        styles.left = 'auto';
      } else if (this.value && this.reverse) {
        styles.left = `${this.height - 3}px`;
        styles.right = 'auto';
      }

      return styles;
    }

  },

  methods: {
    toggle() {
      if (this.disabled) {
        return;
      }
      
      this.value = !this.value;
    }
  }

});
</script>

<template>
  <span
    class="vue-toggles"
    :style="bgStyle"
    role="switch"
    tabindex="0"
    :aria-checked="value ? 'true' : 'false'"
    :aria-readonly="disabled ? 'true' : 'false'"
    @click="toggle"
    @keyup.enter.prevent="toggle"
    @keyup.space.prevent="toggle"
  >
    <span aria-hidden="true" :style="dotStyle" class="dot">
      <span v-show="checkedText && value" :style="textStyle" class="text">
        {{ checkedText }}
      </span>

      <span v-show="uncheckedText && !value" :style="textStyle" class="text">
        {{ uncheckedText }}
      </span>
    </span>
  </span>
</template>

<style>
.vue-toggles {
  display: flex;
  align-items: center;
  border-radius: 9999px;
  overflow: hidden;
  transition: background-color ease 0.2s, width ease 0.2s, height ease 0.2s;
}

.vue-toggles .dot {
  position: relative;
  display: flex;
  align-items: center;
  border-radius: 9999px;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  transition: margin ease 0.2s;
}

.vue-toggles .text {
  position: absolute;
  font-family: inherit;
  user-select: none;
  white-space: nowrap;
}

@media all and (-ms-high-contrast: none) {
  .vue-toggles .text {
    /* IE11 fix */
    top: 50%;
    transform: translateY(-50%);
  }
}

@media (prefers-reduced-motion) {
  /* disable animations if user have a reduced motion setting */
  .vue-toggles,
  .vue-toggles *,
  .vue-toggles *::before,
  .vue-toggles *::after {
    animation: none !important;
    transition: none !important;
    transition-duration: none !important;
  }
}
</style>
