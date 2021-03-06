<template>
  <button
    :class="[ 'c-btn',
              size ? `c-btn-${size}`: '',
              disabled ? 'c-btn-disabled' : '',
              active ? 'c-btn-hover' : '',
              status ? 'c-btn-on' : '',
              iconOnly ? 'c-btn-icon-only' : '',
              radius ? 'c-btn-radius-' + radius: '',
              full ? 'c-btn-full' : '',
            ]"
    :style="{'border-color': borderColor? borderColor: ''}"
    :type="htmlType"
    @touchstart="touchstartHandle"
    @touchend="touchendHandle"
    @touchcancel="touchcancelHandle"
  >
    <span v-if="icon !== '' " :class="['c-btn-icon', icon]"></span>
    <span v-show="!iconOnly" class="c-btn-content"><slot></slot></span>
  </button>
</template>

<script>
export default {
  name: 'v-button',

  data() {
    return {
      active: false,
      status: this.init_status,
      iconOnly: false,
    };
  },

  props: {
    // 按钮大小  small base large full
    size: {
      type: String,
      default: '',
    },
    full: {
      type: Boolean,
      dafault: false,
    },
    // 按钮图标
    icon: {
      type: String,
      required: false,
      default: '',
    },
    // 按钮圆角
    radius: {
      type: String,
      default: '', // circle
    },
    // 是否禁用按钮
    disabled: {
      type: Boolean,
      reuqired: false,
      default: false,
    },
    // 原生type类型
    htmlType: {
      type: String,
      default: 'button',
    },
    // 给添加一个borderColor, 进行颜色配置;
    borderColor: {
      type: String,
      required: false,
      default: '',
    },
  },

  mounted() {
    if (!Object.hasOwnProperty.call(this.$slots, 'default') && this.icon) {
      this.iconOnly = true;
    }
  },

  methods: {
    touchstartHandle() {
      // 按钮被禁用
      if (this.disabled) {
        this.active = false;
        return;
      }
      this.active = true;
    },

    touchendHandle() {
      // 按钮禁用状态
      if (this.disabled) {
        // 按钮禁用后解除按下的状态
        this.active = false;
        return;
      }
      // 解除按下状态
      this.active = false;
    },

    touchcancelHandle() {
      this.touchendHandle();
    },
  },
};

</script>

<style lang="postcss">
@import '../styles/default-theme/variables.css';
@import '../styles/mixins.css';

.c-btn {
  display: inline-flex;
  padding: $btn-padding-base;

  @mixin border;
  border-color: $c-primary;
  border-radius: $btn-radius-sm;

  font-size: $btn-fontsize-base;
  line-height: $btn-fontsize-base;
  color: $c-primary;

  cursor: pointer;
  -webkit-tap-highlight-color:rgba(0, 0, 0, 0);

  /**
   * 解决 Android 4.2 版本 border-radius 和 background 的 Bug
   *
   * background-clip: padding-box;
   *
   * 但是造成了更多手机背景色与border之间出现空隙的问题
   */
  background-color: transparent;

  @mixin transition;

  span {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    display: block;
    width: 100%;
    text-align: center;
    white-space: nowrap;
  }

  &.c-btn-disabled {
    color: $btn-disabled;
    border-color: $btn-disabled;
    /*background-color: $btn-disabled;*/
  }

  &.c-btn-full {
    width: 100%;
  }
}

.c-btn-hover{
  background: $btn-hover;
}

.c-btn-small {
  font-size: $btn-fontsize-sm;
  line-height: $btn-fontsize-sm;

  padding: $btn-padding-sm;
}

.c-btn-large {
  font-size: $btn-fontsize-lg;
  line-height: $btn-fontsize-lg;

  padding: $btn-padding-lg;
}

.c-btn-radius-circle {
  border-radius: 999rem;
  /* padding-top: .08rem; */
  /* padding-bottom: 0.08rem; */
  /* min-width: 0.9rem; */
}

.c-btn-content {
  vertical-align: middle;
}

.c-btn-icon{
  display: inline-block;

  vertical-align: middle;
  /*font-size: calc($font-size-base + 4px);*/

  &:before{
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  + span {
    margin-left: 0.05rem;
  }
}

.c-btn-icon-only {

  padding: 0;
  border-radius: 9999px;
  width: $btn-icon-only-width-base;
  height: $btn-icon-only-width-base;
  font-size: .16rem;
  vertical-align: top;

  &.c-btn-small {
    width: $btn-icon-only-width-sm;
    height: $btn-icon-only-width-sm;
    font-size: .12rem;
  }

  &.c-btn-large {
    width: $btn-icon-only-width-lg;
    height: $btn-icon-only-width-lg;
    font-size: .22rem;
  }
}

.c-btn-group {

  display: flex;

  .c-btn:first-child {
    border-right: none;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }

  .c-btn:last-child {
    border-left-width: 1px;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }

  .c-btn:not(:first-child):not(:last-child){
    border-left-width: 1px;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;

    border-right: none;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }

}
</style>
