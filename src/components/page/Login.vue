<template>
    <div class="login-wrap">
        <div class="bg-circle circle-one"></div>
        <div class="bg-circle circle-two"></div>
        <div class="bg-circle circle-three"></div>

        <div class="login-card">
            <div class="login-header">
                <div class="logo-mark">
                    <span></span>
                </div>
                <div class="login-title">SW Metrics</div>
                <div class="login-subtitle">Software Metrics Management System</div>
            </div>

            <el-form
                :model="param"
                :rules="rules"
                ref="login"
                label-width="0px"
                class="login-form"
            >
                <el-form-item prop="username">
                    <el-input v-model="param.username" placeholder="请输入用户名">
                        <template slot="prefix">
                            <i class="el-icon-lx-people input-icon"></i>
                        </template>
                    </el-input>
                </el-form-item>

                <el-form-item prop="password">
                    <el-input
                        type="password"
                        placeholder="请输入密码"
                        v-model="param.password"
                        @keyup.enter.native="submitForm()"
                    >
                        <template slot="prefix">
                            <i class="el-icon-lx-lock input-icon"></i>
                        </template>
                    </el-input>
                </el-form-item>

                <div class="login-btn">
                    <el-button type="primary" @click="submitForm()">登 录</el-button>
                </div>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            param: {
                username: 'admin',
                password: '123123',
            },
            rules: {
                username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
                password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
            },
        };
    },
    methods: {
        submitForm() {
            this.$refs.login.validate(valid => {
                if (valid) {
                    this.$message.success('登录成功');
                    localStorage.setItem('ms_username', this.param.username);
                    this.$router.push('/');
                } else {
                    this.$message.error('请输入账号和密码');
                    console.log('error submit!!');
                    return false;
                }
            });
        },
    },
};
</script>

<style scoped>
.login-wrap {
    position: relative;
    width: 100%;
    height: 100%;
    min-height: 100vh;
    overflow: hidden;
    background:
        linear-gradient(135deg, #f8fbff 0%, #eef4ff 45%, #e8f0ff 100%);
}

/* 背景装饰 */
.bg-circle {
    position: absolute;
    border-radius: 50%;
    filter: blur(1px);
    opacity: 0.75;
}

.circle-one {
    width: 360px;
    height: 360px;
    left: -130px;
    top: -120px;
    background: radial-gradient(circle, rgba(49, 89, 232, 0.16), rgba(49, 89, 232, 0.03));
}

.circle-two {
    width: 420px;
    height: 420px;
    right: -130px;
    bottom: -150px;
    background: radial-gradient(circle, rgba(131, 104, 246, 0.18), rgba(131, 104, 246, 0.04));
}

.circle-three {
    width: 190px;
    height: 190px;
    right: 20%;
    top: 18%;
    background: radial-gradient(circle, rgba(93, 109, 245, 0.16), rgba(93, 109, 245, 0.03));
}

/* 登录卡片 */
.login-card {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 420px;
    transform: translate(-50%, -50%);
    border-radius: 26px;
    overflow: hidden;
    background: rgba(255, 255, 255, 0.9);
    box-shadow:
        0 24px 60px rgba(49, 89, 232, 0.15),
        0 8px 24px rgba(61, 88, 150, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.85);
}

/* 顶部标题区 */
.login-header {
    padding: 36px 36px 30px;
    text-align: center;
    color: #ffffff;
    background:
        linear-gradient(135deg, #3159e8 0%, #5d6df5 52%, #8368f6 100%);
}

/* logo 图标 */
.logo-mark {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 56px;
    height: 56px;
    margin: 0 auto 18px;
    border-radius: 18px;
    background: rgba(255, 255, 255, 0.18);
    box-shadow:
        inset 0 0 0 1px rgba(255, 255, 255, 0.26),
        0 8px 18px rgba(0, 0, 0, 0.08);
}

.logo-mark span {
    display: block;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: #ffffff;
    box-shadow: 0 0 0 9px rgba(255, 255, 255, 0.18);
}

.login-title {
    font-size: 30px;
    line-height: 36px;
    font-weight: 700;
    letter-spacing: 0.8px;
}

.login-subtitle {
    margin-top: 8px;
    font-size: 13px;
    color: rgba(255, 255, 255, 0.82);
    letter-spacing: 0.4px;
}

/* 表单区 */
.login-form {
    padding: 36px 38px 38px;
}

.login-form >>> .el-form-item {
    margin-bottom: 22px;
}

/* 输入框 */
.login-form >>> .el-input {
    height: 48px;
}

.login-form >>> .el-input__inner {
    height: 48px;
    line-height: 48px;
    padding-left: 46px;
    border-radius: 14px;
    border: 1px solid #dbe5ff;
    color: #334155;
    font-size: 15px;
    background: #f8fbff;
    transition: all 0.25s ease;
}

.login-form >>> .el-input__inner:hover {
    border-color: #b9c8ff;
}

.login-form >>> .el-input__inner:focus {
    border-color: #5d6df5;
    background: #ffffff;
    box-shadow: 0 0 0 4px rgba(93, 109, 245, 0.12);
}

/* 修复 prefix 图标位置，避免超出输入框 */
.login-form >>> .el-input__prefix {
    left: 0;
    width: 46px;
    height: 48px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.input-icon {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 46px;
    height: 48px;
    line-height: 48px;
    font-size: 19px;
    color: #5d6df5;
}

/* 登录按钮 */
.login-btn {
    margin-top: 8px;
}

.login-btn button {
    width: 100%;
    height: 48px;
    border: none;
    border-radius: 15px;
    font-size: 16px;
    font-weight: 600;
    letter-spacing: 2px;
    background: linear-gradient(135deg, #3159e8 0%, #5d6df5 52%, #8368f6 100%);
    box-shadow: 0 12px 24px rgba(49, 89, 232, 0.26);
    transition: all 0.25s ease;
}

.login-btn button:hover {
    transform: translateY(-2px);
    box-shadow: 0 16px 30px rgba(49, 89, 232, 0.34);
    background: linear-gradient(135deg, #2d52dd 0%, #5364ee 52%, #785ff0 100%);
}

.login-btn button:active {
    transform: translateY(0);
}

/* 小屏适配 */
@media screen and (max-width: 520px) {
    .login-card {
        width: calc(100% - 40px);
    }

    .login-form {
        padding: 32px 28px 34px;
    }

    .login-header {
        padding: 32px 28px 26px;
    }
}
</style>
