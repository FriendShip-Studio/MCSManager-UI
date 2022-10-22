<!--
  Copyright (C) 2022 RainChen <asak@irain.cc>
-->

<template>
  <div
    id="register-layer-top"
    :class="{ 'register-layer-fadeout-top': close, 'register-layer-fadein-top': !close }"
  ></div>
  <div
    id="register-layer-left"
    :class="{ 'register-layer-fadeout-left': close, 'register-layer-fadein-left': !close }"
  ></div>
  <div
    id="register-layer-right"
    :class="{ 'register-layer-fadeout-right': close, 'register-layer-fadein-right': !close }"
  ></div>
  <div
    id="register-layer-bottom"
    :class="{ 'register-layer-fadeout-bottom': close, 'register-layer-fadein-bottom': !close }"
  ></div>

  <div id="register-panel-wrapper" :class="{ 'register-panel-wrapper-out': closeWindow }">
    <Panel id="register-panel" body-style="padding: 44px;" v-loading="loading">
      <template #default>
        <div id="register-wrapper">
          <div id="image-wrapper">
            <el-image style="width: 200px;"
                      src="https://cdn.friendship.org.cn/LightPicture/2022/10/a51bb07f4a48dd1d.gif" :fit="fit"/>
          </div>
          <div id="form-wrapper">
            <div id="register-title">{{ $t('register.title') }}</div>
            <div id="register-info">{{ $t('register.info') }}</div>
            <div id="register-notice">
              {{ $t('register.notice-start') }}
              <el-tag size="small">{{ userInfo.userName }}</el-tag>
              {{ $t('register.notice-end') }}
            </div>
            <div style="margin-top: 22px">
              <div class="register-btn-wrapper row-mt">
                <el-link type="primary" @click="handleReturn">{{ $t('register.return') }}</el-link>
                <el-button
                  type="primary"
                  size="small"
                  style="width: 110px"
                  @click="register"
                  :disabled="close"
                  :loading="loading"
                >
                  {{ $t('register.continue') }}
                </el-button>
              </div>
              <div class="register-info-wrapper row-mt" v-if="registerInfo">
                <span class="color-gray">
                  {{ registerInfo }}
                </span>
              </div>
              <div class="register-info-wrapper row-mt">
                <div>
                  <span class="color-gray"
                  >Powered by
                    <a target="black" href="https://cloud.friendship.org.cn/">Friendship Cloud</a></span
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </Panel>
  </div>

  <div>
    <el-row :gutter="20">
      <el-col :md="24" :offset="0">
        <Panel>
          <template #default>
            <el-skeleton :rows="8"/>
          </template>
        </Panel>
        <Panel>
          <template #default>
            <el-skeleton :rows="2"/>
          </template>
        </Panel>
        <Panel>
          <template #default>
            <el-skeleton :rows="2"/>
          </template>
        </Panel>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import Panel from '../../components/Panel';
  import { sleep } from '@/app/service/common.js';
  // eslint-disable-next-line no-unused-vars
  // import router from "../router";


  export default {
    components: {Panel},
    data: function () {
      return {
        close: false,
        closeWindow: false,
        loading: false,
        cause: '',
        registerInfo: ''
      };
    },
    computed: {
      userInfo() {
        return this.$store.state.userInfo;
      }
    },
    methods: {
      async handleReturn() {
        this.close = true;
        this.closeWindow = true;
        await sleep(1500);
        this.$router.back();
      }
    },
    async mounted() {
      console.log('Welcome use MCSManager.');
      console.log('Copyright 2022 MCSManager All rights reserved.');
      // Request register copyleft text
    }
  };
</script>

<style scoped>
  .register-panel-wrapper-out {
    opacity: 0;
    z-index: 1;
  }

  .register-layer-fadeout-top {
    top: 0;
    left: 0;
    right: 0;
    bottom: 100vh;
    opacity: 0.2;
  }

  .register-layer-fadein-top {
    bottom: 50vh;
  }

  .register-layer-fadeout-bottom {
    top: 100vh;
    left: 0;
    right: 0;
    bottom: 0;
    opacity: 0.2;
  }

  .register-layer-fadein-bottom {
    top: 50vh;
  }

  .register-layer-fadeout-left {
    top: 0;
    left: 100vw;
    right: 0;
    bottom: 100vh;
    opacity: 0.2;
  }

  .register-layer-fadein-left {
    left: 50vw;
  }

  .register-layer-fadeout-right {
    top: 0;
    right: 100vw;
    left: 0;
    bottom: 100vh;
    opacity: 0.2;
  }

  .register-layer-fadein-right {
    right: 50vw;
  }

  #register-layer-top,
  #register-layer-bottom {
    transition-property: all;
    transition-duration: 1.6s;
    transition-timing-function: cubic-bezier(0.17, 0.99, 0.63, 0.6);
    transition-delay: 0s;
  }

  #register-layer-right,
  #register-layer-left {
    transition-property: all;
    transition-duration: 0.5s;
    transition-timing-function: cubic-bezier(0.17, 0.99, 0.63, 0.6);
    transition-delay: 0s;
  }

  #register-layer-top,
  #register-layer-left,
  #register-layer-right,
  #register-layer-bottom {
    z-index: 998;
    background-color: rgb(228, 228, 228);
    position: fixed;
  }

  #register-layer-top {
    top: 0;
    left: 0;
    right: 0;
  }

  #register-layer-bottom {
    bottom: 0;
    left: 0;
    right: 0;
  }

  #register-layer-left {
    top: 0;
    right: 0;
    bottom: 0;
  }

  #register-layer-right {
    top: 0;
    left: 0;
    bottom: 0;
  }

  #register-panel-wrapper {
    position: fixed;
    z-index: 999;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;

    display: flex;
    align-items: center;

    transition-property: all;
    transition-duration: 1.5s;
    transition-timing-function: cubic-bezier(1, 0.05, 0.84, 0.74);
    transition-delay: 0s;
  }

  .light #register-panel-wrapper {
    background: #e4e4e4;
  }

  #register-panel {
    min-height: 330px;
    width: 720px;
    transition: all 0.4s 0.2s;
  }

  #register-panel:hover {
    /* box-shadow: 0 0px 18px rgba(0, 0, 0, 0.1); */
    border: 1px solid #828487;
    transform: scale(1.04);
  }

  .register-btn-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .register-info-wrapper {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    text-align: right;
  }

  .register-info-wrapper a {
    color: gray;
    text-decoration: underline;
  }

  #register-wrapper {
    display: flex;
  }

  #form-wrapper {
    font-size: 16px;
  }

  #form-wrapper > * {
    margin-bottom: 24px;
  }

  #register-title {
    font-size: 24px;
    font-weight: 600;
  }

  #image-wrapper {
    display: grid;
    place-items: center;
    margin-right: 44px;
  }

  @media (max-width: 900px) {
    #register-panel {
      text-align: center;
      margin: 0;
      padding: 0;
      min-height: 100vh;
      height: 100vh;
      width: 100vw;
      display: flex;
      justify-content: space-around;
      align-items: center;
      border-radius: 0;
    }

    #register-panel:hover {
      border: none;
    }

    #register-wrapper {
      flex-direction: column;
      align-items: center;
    }

    #image-wrapper {
      margin-right: 0;
    }

    .register-btn-wrapper {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: column-reverse;
      text-align: center;
    }

    .register-btn-wrapper > * {
      margin-bottom: 8px;
    }

    .register-info-wrapper {
      text-align: center;
      display: flex;
      justify-content: space-around;
      align-items: center;
    }
  }

</style>
