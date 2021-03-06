<template>
  <div id="app">
    <cuckoo-plus-header v-if="!$route.meta.hideHeader"/>
    <cuckoo-plus-drawer v-if="!$route.meta.hideDrawer && isOAuthUser"/>
    <mu-container :fluid="true" class="app-content" :style="appContentStyle">
      <keep-alive>
        <router-view v-if="$route.meta.keepAlive" />
      </keep-alive>

      <router-view v-if="!$route.meta.keepAlive" />
    </mu-container>
  </div>
</template>

<script lang="ts">
  import { Vue, Component } from 'vue-property-decorator'
  import { Mutation, State, Getter } from 'vuex-class'
  import * as _ from 'underscore'
  import { UiWidthCheckConstants } from '@/constant'
  import Header from '@/components/Header.vue'
  import Drawer from '@/components/Drawer'

  @Component({
    components: {
      'cuckoo-plus-header': Header,
      'cuckoo-plus-drawer': Drawer
    }
  })
  class App extends Vue {

    $route

    @State('appStatus') appStatus

    @Mutation('updateDocumentWidth') updateDocumentWidth

    @Getter('isOAuthUser') isOAuthUser
    @Getter('isMobileMode') isMobileMode

    mounted () {
      window.addEventListener('resize', _.debounce(() => this.updateDocumentWidth(), 200))
    }

    get appContentStyle () {
      if (this.appStatus.isDrawerOpened &&
        !this.$route.meta.hideDrawer &&
        this.isOAuthUser && !this.isMobileMode) {
        return {
          paddingLeft: `${UiWidthCheckConstants.DRAWER_DESKTOP_WIDTH}px`
        }
      }
    }

  }

  export default App
</script>

<style lang="less" scoped>
  .app-content {
    padding: 56px 0 0 0;
    -webkit-transition: padding-left .45s cubic-bezier(.23,1,.32,1);
    -moz-transition: padding-left .45s cubic-bezier(.23,1,.32,1);
    -ms-transition: padding-left .45s cubic-bezier(.23,1,.32,1);
    -o-transition: padding-left .45s cubic-bezier(.23,1,.32,1);
    transition: padding-left .45s cubic-bezier(.23,1,.32,1);
  }

  @media (min-width: 600px) {
    .app-content {
      padding: 64px 0 0 0;
    }
  }
</style>

<style lang="less">
  body {
    height: 100%;
  }

  a, .mu-load-more {
    -webkit-user-select: auto;
    -moz-user-select: auto;
    -ms-user-select: auto;
    user-select: auto;
  }

  // header z-index 20141223
  // drawer z-index 20141224

  .mu-loading-wrap {
    z-index: 20141222 !important;
  }

  .custom-emoji {
    width: 20px;
    height: 20px;
    vertical-align: text-bottom;
  }
</style>
