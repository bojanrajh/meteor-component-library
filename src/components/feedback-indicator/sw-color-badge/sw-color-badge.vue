<template>
  <span
    class="sw-color-badge"
    :class="variantClass"
    v-bind="$attrs"
    :style="colorStyle"
  >
    <slot />
  </span>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'SwColorBadge',
  props: {
    variant: {
      type: String,
      required: false,
      default: 'default',
      validator(value: string) {
        return [
          'default',
          'warning',
          'critical',
          'positive',
          'info'
        ].includes(value)
      }
    },
    color: {
      type: String,
      required: false,
      default: '',
    },
    rounded: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  computed: {
    colorStyle(): string {
      if (!this.color.length) {
        return '';
      }
      return `background:${this.color}`;
    },
    variantClass(): Record<string, boolean> {
      return {
        [`is--${this.variant}`]: true,
        'is--rounded': this.rounded,
      };
    },
  },
});
</script>

<style lang="scss">
@import "../../assets/scss/variables.scss";

$sw-color-badge-color-fallback: $color-gray-300;
$sw-color-badge-color-warning: $color-pumpkin-spice-500;
$sw-color-badge-color-critical: $color-crimson-500;
$sw-color-badge-color-positive: $color-emerald-500;
$sw-color-badge-color-info: $color-shopware-brand-500;

.sw-color-badge {
  display: inline-block;
  height: 8px;
  width: 8px;
  margin: 0 0 1px 10px;
  border-radius: 2px;
  background-color: $sw-color-badge-color-fallback;

  &.is--rounded {
    border-radius: 100%;
  }

  &.is--warning {
    background-color: $sw-color-badge-color-warning;
  }

  &.is--critical,
  &.is--danger {
    background-color: $sw-color-badge-color-critical;
  }

  &.is--positive {
    background-color: $sw-color-badge-color-positive;
  }

  &.is--info {
    background-color: $sw-color-badge-color-info;
  }
}

</style>
