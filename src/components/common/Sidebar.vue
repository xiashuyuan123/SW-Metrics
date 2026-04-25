<template>
    <div class="sidebar">
        <el-menu
            class="sidebar-el-menu"
            :default-active="onRoutes"
            :collapse="collapse"
            unique-opened
            router
        >
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span slot="title">{{ item.title }}</span>
                        </template>

                        <template v-for="subItem in item.subs">
                            <el-submenu
                                v-if="subItem.subs"
                                :index="subItem.index"
                                :key="subItem.index"
                            >
                                <template slot="title">{{ subItem.title }}</template>
                                <el-menu-item
                                    v-for="(threeItem, i) in subItem.subs"
                                    :key="i"
                                    :index="threeItem.index"
                                >
                                    {{ threeItem.title }}
                                </el-menu-item>
                            </el-submenu>

                            <el-menu-item
                                v-else
                                :index="subItem.index"
                                :key="subItem.index"
                            >
                                {{ subItem.title }}
                            </el-menu-item>
                        </template>
                    </el-submenu>
                </template>

                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i>
                        <span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
import bus from '../common/bus';

export default {
    data() {
        return {
            collapse: false,
            items: [
                {
                    icon: 'el-icon-lx-home',
                    index: 'Class',
                    title: '功能点度量'
                },
                {
                    icon: 'el-icon-lx-cascades',
                    index: 'UserCase',
                    title: '用例点度量'
                },
                {
                    icon: 'el-icon-lx-copy',
                    index: 'SourceCode',
                    title: '源代码度量'
                },
                {
                    icon: 'el-icon-s-open',
                    index: 'InfoFlow',
                    title: '信息流度量'
                },
                {
                    icon: 'el-icon-lx-emoji',
                    index: 'ControlFlow',
                    title: '控制流结构度量'
                },
                // {
                //     icon: 'el-icon-pie-chart',
                //     index: 'DataFlow',
                //     title: '数据流结构度量'
                // },
                {
                    icon: 'el-icon-lx-global',
                    index: 'OOMMetrics',
                    title: '面向对象度量'
                }
            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on('collapse', msg => {
            this.collapse = msg;
            bus.$emit('collapse-content', msg);
        });
    }
};
</script>

<style scoped>
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: auto;
    overflow-x: hidden;
    background: linear-gradient(180deg, #f8fbff 0%, #edf4ff 48%, #e8f0ff 100%);
    box-shadow: 8px 0 24px rgba(61, 88, 150, 0.13);
    border-right: 1px solid rgba(120, 150, 200, 0.18);
}

.sidebar::-webkit-scrollbar {
    width: 0;
}

.sidebar > ul {
    height: 100%;
}

/* 菜单整体 */
.sidebar-el-menu {
    height: 100%;
    padding: 18px 14px;
    box-sizing: border-box;
    border-right: none;
    background: transparent !important;
}

/* 展开状态宽度 */
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}

/* 菜单按钮基础样式：展开和折叠共用统一尺寸 */
.sidebar-el-menu >>> .el-menu-item,
.sidebar-el-menu >>> .el-submenu__title {
    position: relative;
    height: 54px;
    line-height: 54px;
    margin: 10px 0;
    padding: 0 18px !important;
    border-radius: 16px;
    font-size: 16px;
    color: #52627a !important;
    background: transparent !important;
    letter-spacing: 0.4px;
    transition: color 0.25s ease, background 0.25s ease, box-shadow 0.25s ease;
}

/* 图标样式：展开和折叠保持同样大小 */
.sidebar-el-menu >>> .el-menu-item i,
.sidebar-el-menu >>> .el-submenu__title i {
    width: 24px;
    height: 54px;
    line-height: 54px;
    margin-right: 13px;
    font-size: 22px;
    color: #7a8aa3 !important;
    text-align: center;
    transition: color 0.25s ease;
}

/* 鼠标悬浮 */
.sidebar-el-menu >>> .el-menu-item:hover,
.sidebar-el-menu >>> .el-submenu__title:hover {
    color: #3159e8 !important;
    background: rgba(70, 115, 255, 0.1) !important;
}

/* 悬浮时图标 */
.sidebar-el-menu >>> .el-menu-item:hover i,
.sidebar-el-menu >>> .el-submenu__title:hover i {
    color: #3159e8 !important;
}

/* 当前选中菜单 */
.sidebar-el-menu >>> .el-menu-item.is-active {
    color: #ffffff !important;
    font-weight: 600;
    background: linear-gradient(135deg, #3159e8 0%, #6c63ff 100%) !important;
    box-shadow: 0 10px 22px rgba(49, 89, 232, 0.28);
}

/* 当前选中图标 */
.sidebar-el-menu >>> .el-menu-item.is-active i {
    color: #ffffff !important;
}

/* 当前选中项左侧小装饰 */
.sidebar-el-menu >>> .el-menu-item.is-active::before {
    content: '';
    position: absolute;
    left: 8px;
    top: 50%;
    width: 4px;
    height: 22px;
    border-radius: 4px;
    background: #ffffff;
    transform: translateY(-50%);
}

/* 子菜单背景 */
.sidebar-el-menu >>> .el-menu {
    background: transparent !important;
}

/* 子菜单项 */
.sidebar-el-menu >>> .el-submenu .el-menu-item {
    height: 46px;
    line-height: 46px;
    margin: 7px 0;
    padding-left: 46px !important;
    font-size: 14px;
    border-radius: 12px;
}

/* 子菜单箭头 */
.sidebar-el-menu >>> .el-submenu__icon-arrow {
    color: #8b9bb3 !important;
}

/* 折叠状态 */
.sidebar-el-menu.el-menu--collapse {
    width: 70px;
    padding: 16px 0;
    box-sizing: border-box;
}

/* 折叠状态下菜单按钮：只改变宽度，不改变高度和图标大小 */
.sidebar-el-menu.el-menu--collapse >>> .el-menu-item,
.sidebar-el-menu.el-menu--collapse >>> .el-submenu__title {
    width: 54px;
    min-width: 54px;
    height: 54px;
    line-height: 54px;
    margin: 12px auto;
    padding: 0 !important;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 16px;
}

/* 折叠状态下图标：保持和展开状态一致，只调整居中 */
.sidebar-el-menu.el-menu--collapse >>> .el-menu-item i,
.sidebar-el-menu.el-menu--collapse >>> .el-submenu__title i {
    width: 24px !important;
    height: 54px !important;
    line-height: 54px !important;
    margin: 0 !important;
    padding: 0 !important;
    font-size: 22px !important;
    text-align: center;
    transform: translateX(-2px);
}

/* 折叠状态隐藏文字 */
.sidebar-el-menu.el-menu--collapse >>> .el-menu-item span,
.sidebar-el-menu.el-menu--collapse >>> .el-submenu__title span {
    display: none;
}

/* 折叠状态选中项去掉左侧装饰，避免错位 */
.sidebar-el-menu.el-menu--collapse >>> .el-menu-item.is-active::before {
    display: none;
}
</style>
