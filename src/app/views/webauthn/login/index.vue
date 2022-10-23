<!--
  Copyright (C) 2022 RainChen <asak@irain.cc>
-->

<template>
  <div
    id="login-layer-top"
    :class="{ 'login-layer-fadeout-top': close, 'login-layer-fadein-top': !close }"
  ></div>
  <div
    id="login-layer-left"
    :class="{ 'login-layer-fadeout-left': close, 'login-layer-fadein-left': !close }"
  ></div>
  <div
    id="login-layer-right"
    :class="{ 'login-layer-fadeout-right': close, 'login-layer-fadein-right': !close }"
  ></div>
  <div
    id="login-layer-bottom"
    :class="{ 'login-layer-fadeout-bottom': close, 'login-layer-fadein-bottom': !close }"
  ></div>

  <div id="login-panel-wrapper" :class="{ 'login-panel-wrapper-out': closeWindow }">
    <Panel id="login-panel" body-style="padding: 44px;" v-loading="loading">
      <template #default>
        <div id="login-wrapper" class="main-wrapper" v-if="!isFailed">
          <div class="login-title">{{ $t("webauthn.login.loading") }}</div>
          <div class="login-info">{{ $t("webauthn.login.loadinginfo") }}</div>
          <div class="login-progress">
            <el-progress
              :show-text="false"
              :percentage="75"
              :format="format"
              :indeterminate="true"
            />
          </div>
        </div>
        <div id="error-wrapper" class="main-wrapper" v-if="isFailed">
          <div class="login-title">{{ $t("webauthn.error.title") }}</div>
          <div class="login-info">{{ $t("webauthn.login.loadinginfo") }}</div>
          <div class="login-progress">
            <el-progress
              :show-text="false"
              :percentage="75"
              :format="format"
              :indeterminate="true"
              status="exception"
            />
          </div>
        </div>
        <el-button size="small" type="danger" plain @click="isFailed = !isFailed">
          这个按钮用来切换isFailed
        </el-button>
      </template>
    </Panel>
  </div>
  <div>
    <el-row :gutter="20">
      <el-col :md="24" :offset="0">
        <Panel>
          <template #default>
            <el-skeleton :rows="8" />
          </template>
        </Panel>
        <Panel>
          <template #default>
            <el-skeleton :rows="2" />
          </template>
        </Panel>
        <Panel>
          <template #default>
            <el-skeleton :rows="2" />
          </template>
        </Panel>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Panel from "@/components/Panel";
import { sleep } from "@/app/service/common.js";

// eslint-disable-next-line no-unused-vars
// import router from "../router";
export default {
  components: { Panel },
  data: function () {
    return {
      close: false,
      closeWindow: false,
      loading: false,
      isFailed: false
    };
  },
  methods: {
    async handleReturn() {
      this.close = true;
      this.closeWindow = true;
      await sleep(1500);
      this.$router.back();
    }
  }
};
</script>

<style scoped>
.login-panel-wrapper-out {
  opacity: 0;
  z-index: 1;
}

.login-layer-fadeout-top {
  top: 0;
  left: 0;
  right: 0;
  bottom: 100vh;
  opacity: 0.2;
}

.login-layer-fadein-top {
  bottom: 50vh;
}

.login-layer-fadeout-bottom {
  top: 100vh;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 0.2;
}

.login-layer-fadein-bottom {
  top: 50vh;
}

.login-layer-fadeout-left {
  top: 0;
  left: 100vw;
  right: 0;
  bottom: 100vh;
  opacity: 0.2;
}

.login-layer-fadein-left {
  left: 50vw;
}

.login-layer-fadeout-right {
  top: 0;
  right: 100vw;
  left: 0;
  bottom: 100vh;
  opacity: 0.2;
}

.login-layer-fadein-right {
  right: 50vw;
}

#login-layer-top,
#login-layer-bottom {
  transition-property: all;
  transition-duration: 1.6s;
  transition-timing-function: cubic-bezier(0.17, 0.99, 0.63, 0.6);
  transition-delay: 0s;
}

#login-layer-right,
#login-layer-left {
  transition-property: all;
  transition-duration: 0.5s;
  transition-timing-function: cubic-bezier(0.17, 0.99, 0.63, 0.6);
  transition-delay: 0s;
}

#login-layer-top,
#login-layer-left,
#login-layer-right,
#login-layer-bottom {
  z-index: 998;
  background-color: rgb(228, 228, 228);
  position: fixed;
}

#login-layer-top {
  top: 0;
  left: 0;
  right: 0;
}

#login-layer-bottom {
  bottom: 0;
  left: 0;
  right: 0;
}

#login-layer-left {
  top: 0;
  right: 0;
  bottom: 0;
}

#login-layer-right {
  top: 0;
  left: 0;
  bottom: 0;
}

#login-panel-wrapper {
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

.light #login-panel-wrapper {
  background: #e4e4e4;
}

#login-panel {
  min-height: 230px;
  width: 450px;
  transition: all 0.4s 0.2s;
}

.main-wrapper {
  font-size: 16px;
}

.main-wrapper > * {
  margin-bottom: 24px;
}

.login-title {
  font-size: 24px;
  font-weight: 600;
}

.login-progress {
  margin-top: 50px;
  width: 100%;
}
</style>
