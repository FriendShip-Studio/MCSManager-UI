<template>
  <Panel id="register-panel" body-style="padding: 44px;" v-loading="loading">
    <template #default>
      <div id="register-wrapper">
        <div id="form-wrapper">
          <div id="register-title">{{ $t("webauthn.register.title") }}</div>
          <div id="register-info">{{ $t("webauthn.register.info") }}</div>
          <div id="register-notice">
            这里开始写登录的内容呃
          </div>
          <div style="margin-top: 22px">
            <div class="register-btn-wrapper row-mt">
              <el-link type="primary" @click="handleReturn">{{
              $t("webauthn.register.return")
              }}</el-link>
              <el-button type="primary" size="small" style="width: 110px" :disabled="close" :loading="loading"
                @click="handleContinue">
                {{ $t("webauthn.register.continue") }}
              </el-button>
            </div>
          </div>
        </div>
      </div>
    </template>
  </Panel>
</template>
<script>
import Panel from "@/components/Panel";
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
    return {
      loading: false
    };
  },
  methods: {
    handleContinue: async function () {
      this.loading = true;
      //这里发送你的的一个请求，然后调用setContext方法，将请求的结果传递给父组件
      this.setContext({ a: 1 });
      this.processStep("processing");
      this.loading = false;
    }
  }
};
</script>
<style lang="css">
#register-panel {
  min-height: 330px;
  width: 720px;
  transition: all 0.4s 0.2s;
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

#form-wrapper>* {
  margin-bottom: 24px;
}

#register-title {
  font-size: 24px;
  font-weight: 600;
}

#image-wrapper {
  display: grid;
  padding-top: 2%;
  align-items: start;
  justify-items: center;
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

  .register-btn-wrapper>* {
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
