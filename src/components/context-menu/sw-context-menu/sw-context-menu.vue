<template>
  <div class="sw-context-menu">
    <div class="sw-context-menu__content">
      <slot />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'SwContextMenu',
});
</script>

<style lang="scss">
@import '../../assets/scss/variables.scss';

$sw-context-menu-color-border:           $color-gray-300;
$sw-context-menu-color-background-hover: $color-gray-100;
$sw-context-menu-color-text:             $color-darkgray-200;
$sw-context-menu-color-active:           $color-shopware-brand-500;
$sw-context-menu-z-index:                $z-index-context-menu;

.sw-context-menu {
  position: relative;
  z-index: $sw-context-menu-z-index;

  &.is--top-align {
    .sw-context-menu__content {
      margin-top: calc(-100% - 30px);

      &::before {
        transform: rotate(-135deg);
        bottom: -5px;
        top: unset;
      }
    }
  }

  .sw-context-menu__content {
    padding: 8px;
    background: $color-white;
    box-shadow: 0 3px 6px 0 rgba(120, 138, 155, 0.3);
    border: 1px solid $sw-context-menu-color-border;
    border-radius: 4px;
    text-align: left;
    font-size: 14px;
    color: $sw-context-menu-color-text;
    position: relative;

    &::before {
      content: '';
      position: absolute;
      top: -5px;
      left: 28px;
      height: 8px;
      width: 8px;
      transform: rotate(45deg);
      background: $color-white;
      border-top: 1px solid $sw-context-menu-color-border;
      border-left: 1px solid $sw-context-menu-color-border;
    }

    .currency-symbol {
      font-weight: bold;
      font-size: 18px;
    }

    .sw-context-menu__group {
      padding-top: 10px;
      margin-top: 10px;
      border-top: 1px solid $sw-context-menu-color-border;

      &:first-child {
        border-top: 0 none;
        padding-top: 5px;
        margin-top: 0;
      }
    }
  }
}
</style>
