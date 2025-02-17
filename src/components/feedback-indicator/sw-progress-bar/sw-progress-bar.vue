<template>
  <sw-base-field
    class="sw-progress-bar"
    role="progressbar"
    :aria-valuenow="value"
    :aria-valuemax="maxValue"
    aria-label="Current progress"
    :has-focus="false"
  >
    <template #label>
      {{ label }}

      <span class="sw-progress-bar__progress-label">
        {{ progressLabel }}
      </span>
    </template>

    <template #element>
      <div class="sw-progress-bar__total">
        <div
          class="sw-progress-bar__value"
          data-testid="progress-bar-value"
          :style="{ width: styleWidth }"
          :class="progressClasses"
        />
      </div>
    </template>

    <template #error>
      <sw-field-error
        v-if="error"
        :error="error"
      />
    </template>
  </sw-base-field>
</template>

<script lang="ts">
import Vue from 'vue';
import SwBaseField from "../../form/_internal/sw-base-field/sw-base-field.vue";
import SwFieldError from "../../form/_internal/sw-field-error/sw-field-error.vue";

export default Vue.extend({
  name: 'SwProgressBar',

  components: {
    'sw-base-field': SwBaseField,
    'sw-field-error': SwFieldError,
  },

  props: {
    /**
     * The current value which is used for showing the current progress.
     */
    value: {
      type: Number,
      default: 0,
    },
    /**
     * The max value sets the value where the progress will be finished.
     */
    maxValue: {
      type: Number,
      default: 100,
      required: false,
    },
    /**
     * A label for the progress bar. Usually used to guide the user what value kind of activity is currently running.
     */
    label: {
      type: String,
      required: false,
      default: null,
    },
    /**
     * Change how the progress label looks like. Examples are "kb", "mb", "items" or more. For percentage just use "percentage"
     * @example "kb"
     */
    progressLabelType: {
      type: String,
      required: false,
      default: 'percent'
    },
    /**
     * An error in your business logic related to this field.
     *
     * @example {"code": 500, "detail": "Error while loading"}
     */
    error: {
      type: Object,
      required: false,
      default: null,
    },
  },

  computed: {
    progressLabel(): string {
      if (!this.progressLabelType || this.progressLabelType === 'percent') {
        return this.styleWidth;
      }

      return `${this.value} ${this.progressLabelType} / ${this.maxValue} ${this.progressLabelType}`;
    },

    styleWidth(): string {
      // @ts-expect-error - vue can't detect value correctly
      let percentage = parseInt(this.value / this.maxValue * 100);

      if (percentage > 100) {
        percentage = 100;
      }

      if (percentage < 0) {
        percentage = 0;
      }

      return `${percentage}%`;
    },

    progressClasses(): {
      'sw-progress-bar__value--no-transition': boolean,
      'sw-progress-bar__value--has-error': boolean,
    } {
      return {
        'sw-progress-bar__value--no-transition': this.value < 1 || this.value >= this.maxValue,
        'sw-progress-bar__value--has-error': !!this.error,
      };
    },
  },
});
</script>

<style lang="scss">
@use "sass:math";
@import "../../assets/scss/variables.scss";

$sw-progress-bar-height: 8px;

.sw-progress-bar {
  .sw-block-field__block {
    border: none;
  }

  label {
    display: flex;
  }

  &__progress-label {
    margin-left: auto;
  }

  .sw-progress-bar__total {
    width: 100%;
    height: $sw-progress-bar-height;
    background-color: $color-gray-200;
    border-radius: math.div($sw-progress-bar-height, 2);
  }

  .sw-progress-bar__value {
    transition: 1s width linear;
    height: 100%;
    background-color: $color-shopware-brand-500;
    border-radius: math.div($sw-progress-bar-height, 2);

    &--no-transition {
      transition: 0s width linear;
    }

    &--has-error {
      transition: 0s width linear;
      background-color: $color-crimson-500;
    }
  }
}
</style>
