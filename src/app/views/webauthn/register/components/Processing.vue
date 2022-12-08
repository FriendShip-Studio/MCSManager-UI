<!--
  Copyright (C) 2022 RainChen <asak@irain.cc>
-->

<template>
  <div id="reg-layer-top" :class="{ 'reg-layer-fadeout-top': close, 'reg-layer-fadein-top': !close }"></div>
  <div id="reg-layer-left" :class="{ 'reg-layer-fadeout-left': close, 'reg-layer-fadein-left': !close }"></div>
  <div id="reg-layer-right" :class="{ 'reg-layer-fadeout-right': close, 'reg-layer-fadein-right': !close }"></div>
  <div id="reg-layer-bottom" :class="{ 'reg-layer-fadeout-bottom': close, 'reg-layer-fadein-bottom': !close }">
  </div>

  <div id="reg-panel-wrapper" :class="{ 'reg-panel-wrapper-out': close }">
    <Transition name="slide" mode="out-in" appear>
      <Panel id="reg-panel" v-loading="loading" body-style="padding: 0;" v-if="(!isFailed && !isSuccess)">
        <template #default>
          <div id="body-wrapper">
            <div id="reg-wrapper" class="main-wrapper">
              <div class="reg-title">{{ $t("webauthn.register.loading") }}</div>
              <div class="reg-info">{{ $t("webauthn.register.loadinginfo") }}</div>
              <div class="reg-progress">
                <el-progress :show-text="false" :percentage="100" :indeterminate="true" />
              </div>
            </div>
          </div>
        </template>
      </Panel>
      <Panel id="error-panel" v-loading="loading" body-style="padding: 0;" v-else-if="isFailed">
        <template #default>
          <div id="body-wrapper">
            <div id="error-wrapper" class="error-wrapper">
              <i class="el-icon-cirle-close-filled"></i>
              <div class="error-title">{{ $t("webauthn.register.error.title") }}</div>
              <el-divider />
              <div class="error-info">{{ $t("webauthn.register.error.info_1") }}</div>
              <div class="error-info">{{ $t("webauthn.register.error.info_2") }}</div>
              <div class="error-detail">
                <p id="regError" class="error">{{ errorTip }}</p>
              </div>
              <div class="error-contact">
                <span v-html="$t('webauthn.register.error.contact')">
                </span>
              </div>
              <div class="error-return">
                <el-button type="warning" @click="handleReturn">返回</el-button>
              </div>
            </div>
          </div>
        </template>
      </Panel>
      <Panel id="success-panel" v-loading="loading" body-style="padding: 0;" v-else-if="isSuccess">
        <template #default>
          <div id="success-wrapper">
            <div id="success-image-wrapper">
              <el-image style="width: 200px"
                src="https://cdn.friendship.org.cn/LightPicture/2022/10/cb39598188d255bd.jpg" fit="contain" />
            </div>
            <div id="form-wrapper">
              <div id="success-title">{{ $t("webauthn.register.success.title") }}</div>
              <div id="success-info">{{ $t("webauthn.register.success.info") }}</div>
              <div style="margin-top: 22px">
                <div class="register-btn-wrapper row-mt">
                  <el-button type="primary" size="small" style="width: 110px" :disabled="close" :loading="loading"
                    @click="handleReturn">
                    {{ $t("webauthn.register.success.finish") }}
                  </el-button>
                </div>
              </div>
            </div>
          </div>
        </template>
      </Panel>
    </Transition>
    <!-- <el-button type="primary" @click="isFailed = !isFailed">
      <span class="rainbow">寄了</span>
    </el-button>
    <el-button type="primary" @click="isSuccess = !isSuccess">
      <span>看看你的</span>
    </el-button> -->
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
import { startRegistration } from "@simplewebauthn/browser";
import { request } from "../../../../service/protocol";
import {
  API_WEBAUTHN_generate_registration_options,
  API_WEBAUTHN_verify_registration,
sleep
} from "../../../../service/common";

export default {
  components: { Panel },
  props: {
    processStep: {
      type: Function,
      default: () => { }
    },
    handleReturn: {
      type: Function,
      default: () => { }
    },
    close: {
      type: Boolean,
      default: false
    },
    setContext: {
      type: Function,
      default: () => { }
    },
    ctx: {
      type: Object,
      default: () => { }
    }
  },
  data: function () {
    this.webauthn();
    return {
      loading: false,
      isFailed: false,
      isSuccess: false,
      errorTip: "未知错误"
    };
  },
  methods: {
    async webauthn() {
      const username = sessionStorage.getItem("username");
      console.log("UserName: ", username);

      await sleep(1500);
      let attResp;
      try {
        const opts = await request({
          method: "POST",
          url: API_WEBAUTHN_generate_registration_options,
          data: {
            username
          }
        });

        // Require a resident key for this demo
        /* opts.authenticatorSelection.residentKey = "discouraged";
        opts.authenticatorSelection.requireResidentKey = false;
        opts.extensions = {
          credProps: true
        }; */
        attResp = await startRegistration(opts);
      } catch (error) {
        if (error.name === "InvalidStateError") {
          this.errorTip = "错误: 此设备已被注册!";
          this.isFailed = !this.isFailed;
        } else {
          this.errorTip = error;
          this.isFailed = !this.isFailed;
        }
        throw error;
      }
      const verificationJSON = await request({
        method: "POST",
        url: API_WEBAUTHN_verify_registration,
        data: {
          username,
          registration_credential_json: attResp
        }
      });
      if (verificationJSON && verificationJSON.verified) {
        this.isSuccess = !this.isSuccess
      } else {
        this.errorTip = `响应错误: ${JSON.stringify(
          verificationJSON
        )}`;
        this.isFailed = !this.isFailed;
      }
    }
  }
};
</script>

<style scoped>
.reg-panel-wrapper-out {
  opacity: 0;
  z-index: 1;
}

.reg-layer-fadeout-top {
  top: 0;
  left: 0;
  right: 0;
  bottom: 100vh;
  opacity: 0.2;
}

.reg-layer-fadein-top {
  bottom: 50vh;
}

.reg-layer-fadeout-bottom {
  top: 100vh;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 0.2;
}

.reg-layer-fadein-bottom {
  top: 50vh;
}

.reg-layer-fadeout-left {
  top: 0;
  left: 100vw;
  right: 0;
  bottom: 100vh;
  opacity: 0.2;
}

.reg-layer-fadein-left {
  left: 50vw;
}

.reg-layer-fadeout-right {
  top: 0;
  right: 100vw;
  left: 0;
  bottom: 100vh;
  opacity: 0.2;
}

.reg-layer-fadein-right {
  right: 50vw;
}

#reg-layer-top,
#reg-layer-bottom {
  transition-property: all;
  transition-duration: 1.6s;
  transition-timing-function: cubic-bezier(0.17, 0.99, 0.63, 0.6);
  transition-delay: 0s;
}

#reg-layer-right,
#reg-layer-left {
  transition-property: all;
  transition-duration: 0.5s;
  transition-timing-function: cubic-bezier(0.17, 0.99, 0.63, 0.6);
  transition-delay: 0s;
}

#reg-layer-top,
#reg-layer-left,
#reg-layer-right,
#reg-layer-bottom {
  z-index: 998;
  background-color: rgb(228, 228, 228);
  position: fixed;
}

#reg-layer-top {
  top: 0;
  left: 0;
  right: 0;
}

#reg-layer-bottom {
  bottom: 0;
  left: 0;
  right: 0;
}

#reg-layer-left {
  top: 0;
  right: 0;
  bottom: 0;
}

#reg-layer-right {
  top: 0;
  left: 0;
  bottom: 0;
}

#reg-panel-wrapper {
  position: fixed;
  z-index: 999;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  transition-property: all;
  transition-duration: 1.5s;
  transition-timing-function: cubic-bezier(1, 0.05, 0.84, 0.74);
  transition-delay: 0s;
}

.light #reg-panel-wrapper {
  background: #e4e4e4;
}

#reg-panel {
  min-height: 230px;
  width: 450px;
}

#error-panel {
  min-height: 460px;
  width: 900px;
  transition: all 0.4s 0.2s;
}

#success-panel {
  min-height: 200px;
  width: 800px;
  transition: all 0.4s 0.2s;
}

#body-wrapper {
  padding: 44px;
  overflow-x: hidden;
}

#success-wrapper {
  padding: 44px;
  display: flex;
}

#form-wrapper {
  font-size: 16px;
}

#form-wrapper>* {
  margin-bottom: 24px;
}

#success-title {
  font-size: 24px;
  font-weight: 600;
}

#success-image-wrapper {
  display: grid;
  padding-top: 5%;
  align-items: start;
  justify-items: center;
  margin-right: 44px;
}

@media (max-width: 900px) {
  #success-panel {
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

  #success-panel:hover {
    border: none;
  }

  #success-wrapper {
    flex-direction: column;
    align-items: center;
  }

  #success-image-wrapper {
    margin-right: 0;
  }
}

.main-wrapper {
  width: 360px;
  flex-shrink: 0;
  font-size: 16px;
}

.error-wrapper {
  width: 100%;
  text-align: center;
  flex-shrink: 0;
  font-size: 16px;
}

.error-title {
  text-align: left;
  font-size: 28px;
  font-weight: 600;
}

.error-info {
  text-align: left;
  font-size: 16px;
  margin-top: 12px;
}

.error-detail {
  text-align: left;
  font-size: 14px;
  margin-top: 12px;
  background-color: rgba(161, 161, 161, 0.295);
  padding: 15px;
  border: 1px solid --el-box-shadow-lighter;
  border-radius: 4px;
}

.main-wrapper>* {
  margin-bottom: 24px;
}

.error-return {
  text-align: right;
  margin-top: 12px;
}

.error-contact {
  text-align: left;
  margin-top: 14px;
  font-size: small;
}

.reg-title {
  font-size: 24px;
  font-weight: 600;
}

.reg-progress {
  margin-top: 50px;
  width: 100%;
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease-in-out;
}

.slide-enter-active {
  position: relative;
  visibility: visible;
}

.slide-leave-active {
  position: relative;
  visibility: hidden;
}

.slide-enter-from {
  transform: translateX(120px);
  opacity: 0;
}

.slide-enter-to,
.slide-leave-from {
  transform: translateX(0);
  opacity: 1;
}

.slide-leave-to {
  transform: translateX(-120px);
  opacity: 0;
}

.rainbow {
  background-image: linear-gradient(to right,
      orangered,
      orange,
      gold,
      lightgreen,
      cyan,
      dodgerblue,
      mediumpurple,
      hotpink,
      orangered);
  background-size: 110vw;
  animation: sliding 30s linear infinite;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: bold;
}

@keyframes sliding {
  to {
    background-position: -2000vw;
  }
}
</style>
