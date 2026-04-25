<template>
    <div class="header">
        <!-- 折叠按钮 -->
        <div class="collapse-btn" @click="collapseChage">
            <i v-if="!collapse" class="el-icon-s-fold"></i>
            <i v-else class="el-icon-s-unfold"></i>
        </div>

        <div class="logo">
            <span class="logo-dot"></span>
            <span>SW Metrics</span>
        </div>

        <div class="header-right">
            <div class="header-user-con">
                <!-- 全屏显示 -->
                <div class="btn-fullscreen" @click="handleFullScreen">
                    <el-tooltip effect="dark" :content="fullscreen ? `取消全屏` : `全屏`" placement="bottom">
                        <i class="el-icon-rank"></i>
                    </el-tooltip>
                </div>

                <!-- 用户头像 -->
                <div class="user-avator">
                    <img src="../../assets/img/img.png" />
                </div>

                <!-- 用户名下拉菜单 -->
                <el-dropdown class="user-name" trigger="click" @command="handleCommand">
                    <span class="el-dropdown-link">
                        {{ username }}
                        <i class="el-icon-caret-bottom"></i>
                    </span>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item command="loginout">退出登录</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
            </div>
        </div>
    </div>
</template>

<script>
import bus from '../common/bus';

export default {
    data() {
        return {
            collapse: false,
            fullscreen: false,
            name: 'linxin'
        };
    },
    computed: {
        username() {
            let username = localStorage.getItem('ms_username');
            return username ? username : this.name;
        }
    },
    methods: {
        // 用户名下拉菜单选择事件
        handleCommand(command) {
            if (command == 'loginout') {
                localStorage.removeItem('ms_username');
                this.$router.push('/login');
            }
        },

        // 侧边栏折叠
        collapseChage() {
            this.collapse = !this.collapse;
            bus.$emit('collapse', this.collapse);
        },

        // 全屏事件
        handleFullScreen() {
            let element = document.documentElement;

            if (this.fullscreen) {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.webkitCancelFullScreen) {
                    document.webkitCancelFullScreen();
                } else if (document.mozCancelFullScreen) {
                    document.mozCancelFullScreen();
                } else if (document.msExitFullscreen) {
                    document.msExitFullscreen();
                }
            } else {
                if (element.requestFullscreen) {
                    element.requestFullscreen();
                } else if (element.webkitRequestFullScreen) {
                    element.webkitRequestFullScreen();
                } else if (element.mozRequestFullScreen) {
                    element.mozRequestFullScreen();
                } else if (element.msRequestFullscreen) {
                    element.msRequestFullscreen();
                }
            }

            this.fullscreen = !this.fullscreen;
        }
    },
    mounted() {
        if (document.body.clientWidth < 1500) {
            this.collapseChage();
        }
    }
};
</script>

<style scoped>
.header {
    position: relative;
    box-sizing: border-box;
    width: 100%;
    height: 70px;
    font-size: 22px;
    color: #ffffff;
    background: linear-gradient(120deg, #3159e8 0%, #5d6df5 45%, #8368f6 100%);
    box-shadow: 0 8px 26px rgba(74, 97, 238, 0.28);
    z-index: 20;
}

/* 折叠按钮 */
.collapse-btn {
    float: left;
    width: 70px;
    height: 70px;
    padding: 0;
    cursor: pointer;
    line-height: 70px;
    text-align: center;
    transition: all 0.25s ease;
}

.collapse-btn i {
    font-size: 24px;
    color: #ffffff;
}

.collapse-btn:hover {
    background: rgba(255, 255, 255, 0.16);
}

/* 系统名称 */
.header .logo {
    float: left;
    width: 250px;
    height: 70px;
    line-height: 70px;
    display: flex;
    align-items: center;
    font-size: 25px;
    font-weight: 700;
    letter-spacing: 0.6px;
    color: #ffffff;
    text-shadow: 0 3px 10px rgba(0, 0, 0, 0.12);
}

/* logo 前的小圆点装饰 */
.logo-dot {
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 12px;
    border-radius: 50%;
    background: #ffffff;
    box-shadow: 0 0 0 6px rgba(255, 255, 255, 0.18);
}

/* 右侧区域 */
.header-right {
    float: right;
    padding-right: 36px;
}

.header-user-con {
    display: flex;
    height: 70px;
    align-items: center;
}

/* 全屏按钮 */
.btn-fullscreen {
    position: relative;
    width: 38px;
    height: 38px;
    margin-left: 12px;
    text-align: center;
    line-height: 38px;
    border-radius: 14px;
    cursor: pointer;
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(8px);
    transition: all 0.25s ease;
    transform: rotate(45deg);
    font-size: 22px;
}

.btn-fullscreen:hover {
    background: rgba(255, 255, 255, 0.26);
    transform: rotate(45deg) translateY(-2px);
}

.btn-fullscreen i {
    color: #ffffff;
    font-size: 20px;
}

/* 头像 */
.user-avator {
    margin-left: 22px;
}

.user-avator img {
    display: block;
    width: 44px;
    height: 44px;
    padding: 2px;
    border-radius: 50%;
    background: #ffffff;
    box-shadow: 0 8px 18px rgba(0, 0, 0, 0.16);
}

/* 用户名 */
.user-name {
    margin-left: 12px;
}

.el-dropdown-link {
    display: flex;
    align-items: center;
    height: 36px;
    padding: 0 13px;
    color: #ffffff;
    font-size: 15px;
    font-weight: 600;
    border-radius: 18px;
    cursor: pointer;
    background: rgba(255, 255, 255, 0.13);
    transition: all 0.25s ease;
}

.el-dropdown-link:hover {
    background: rgba(255, 255, 255, 0.24);
}

.el-dropdown-link i {
    margin-left: 5px;
}

.el-dropdown-menu__item {
    text-align: center;
}
</style>
