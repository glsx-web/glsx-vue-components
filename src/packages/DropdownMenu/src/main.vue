<!--<template>
    <el-dropdown-menu>
        <slot></slot>
     </el-dropdown-menu>
</template>

<script>
export default {
  name: 'GlDropdownMenu'
}
</script> -->
<template>
  <transition name="el-zoom-in-top" @after-leave="doDestroy">
    <ul class="el-dropdown-menu el-popper" :class="[size && `el-dropdown-menu--${size}`]" v-show="showPopper">
      <slot></slot>
    </ul>
  </transition>
</template>
<script>
  import Popper from 'element-ui/src/utils/vue-popper'

export default {
    name: 'GlDropdownMenu',
    componentName: 'ElDropdownMenu',
    mixins: [Popper],
    props: {
      visibleArrow: {
        type: Boolean,
        default: true
      },
      arrowOffset: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        size: this.dropdown.dropdownSize
      }
    },
    inject: ['dropdown'],

    created() {
      this.$on('updatePopper', () => {
        if (this.showPopper) this.updatePopper()
      })
      this.$on('visible', val => {
        this.showPopper = val
      })
    },
    mounted() {
      this.$parent.popperElm = this.popperElm = this.$el
      this.referenceElm = this.$parent.$el
    },
    watch: {
      'dropdown.placement': {
        immediate: true,
        handler(val) {
          this.currentPlacement = val
        }
      }
    }
  }
</script>
