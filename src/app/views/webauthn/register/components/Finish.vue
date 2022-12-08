<!--
  Copyright (C) 2022 RainChen <asak@irain.cc>
-->

<template>
    <div id="reg-layer-top" :class="{ 'reg-layer-fadeout-top': close, 'reg-layer-fadein-top': !close }"></div>
    <div id="reg-layer-left" :class="{ 'reg-layer-fadeout-left': close, 'reg-layer-fadein-left': !close }"></div>
    <div id="reg-layer-right" :class="{ 'reg-layer-fadeout-right': close, 'reg-layer-fadein-right': !close }"></div>
    <div id="reg-layer-bottom" :class="{ 'reg-layer-fadeout-bottom': close, 'reg-layer-fadein-bottom': !close }">
    </div>

    <div id="finish-panel-wrapper" :class="{ 'reg-panel-wrapper-out': close }">
        <Panel id="reg-panel" v-loading="loading" body-style="padding: 0;">
            <template #default>
                <div id="register-wrapper">
                    <div id="image-wrapper">
                        <el-image style="width: 200px"
                            src="https://cdn.friendship.org.cn/LightPicture/2022/10/cb39598188d255bd.jpg"
                            fit="contain" />
                    </div>
                    <div id="form-wrapper">
                        <div id="register-title">{{ $t("webauthn.register.success.title") }}</div>
                        <div id="register-info">{{ $t("webauthn.register.success.info") }}</div>
                        <div style="margin-top: 22px">
                            <div class="register-btn-wrapper row-mt">
                                <el-button type="primary" size="small" style="width: 110px" :disabled="close"
                                    :loading="loading" @click="finished">
                                    {{ $t("webauthn.register.success.finish") }}
                                </el-button>
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
import router from "../../../../router";

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
        async finished() {
            router.push({ path: "/" });
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

#reg-panel {
    min-height: 230px;
    width: 450px;
}

#error-panel {
    min-height: 460px;
    width: 900px;
    transition: all 0.4s 0.2s;
}

#body-wrapper {
    padding: 44px;
    overflow-x: hidden;
}
</style>
