<template>
    <Panel id="error-panel" body-style="padding: 44px;" v-loading="loading">
        <template #default>
            <div id="error-wrapper">
                <div id="form-wrapper">
                    <div id="error-title">{{ $t("webauthn.error.title") }}</div>
                    <div id="error-info">{{ $t("webauthn.login.loadinginfo") }}</div>
                    <div id="error-progress">
                        <el-progress :show-text="false" :percentage="75" :format="format" :indeterminate="true" />
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
            this.processStep("error");
            this.loading = false;
        }
    }
};
</script>
<style lang="css">
#error-panel {
    min-height: 230px;
    width: 450px;
    transition: all 0.4s 0.2s;
}

.error-btn-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.error-info-wrapper {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    text-align: right;
}

.error-info-wrapper a {
    color: gray;
    text-decoration: underline;
}

#error-wrapper {
    display: flex;
}

#form-wrapper {
    font-size: 16px;
}

#form-wrapper>* {
    margin-bottom: 24px;
}

#error-title {
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

#error-progress {
    margin-top: 50px;
    width: 100%;
}

@media (max-width: 900px) {
    #error-panel {
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

    #error-panel:hover {
        border: none;
    }

    #error-wrapper {
        flex-direction: column;
        align-items: center;
    }

    #image-wrapper {
        margin-right: 0;
    }

    .error-btn-wrapper {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column-reverse;
        text-align: center;
    }

    .error-btn-wrapper>* {
        margin-bottom: 8px;
    }

    .error-info-wrapper {
        text-align: center;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }
}
</style>
