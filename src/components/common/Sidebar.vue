<template>
    <div class='sidebar'>
        <el-menu class='sidebar-el-menu' :default-active='onRoutes' :collapse='collapse' background-color='#324157'
                 text-color='#bfcbd9' active-text-color='#20a0ff' unique-opened router>
            <template v-for='item in items'>
                <template v-if='item.subs'>
                    <el-submenu :index='item.index' :key='item.index'>
                        <template slot='title'>
                            <i :class='item.icon'></i>
                            <span slot='title'>{{ item.title }}</span>
                        </template>
                        <template v-for='subItem in item.subs'>
                            <el-submenu v-if='subItem.subs' :index='subItem.index' :key='subItem.index'>
                                <template slot='title'>{{ subItem.title }}</template>
                                <el-menu-item v-for='(threeItem,i) in subItem.subs' :key='i' :index='threeItem.index'>
                                    {{ threeItem.title }}
                                </el-menu-item>
                            </el-submenu>
                            <el-menu-item v-else :index='subItem.index' :key='subItem.index'>{{ subItem.title }}
                            </el-menu-item>
                        </template>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index='item.index' :key='item.index'>
                        <i :class='item.icon'></i>
                        <span slot='title'>{{ item.title }}</span>
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
            items: []
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    mounted() {
        this.getSideMenu();
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on('collapse', msg => {
            this.collapse = msg;
            bus.$emit('collapse-content', msg);
        });
    },
    methods: {
        getSideMenu: function() {
            if (localStorage.getItem('ms_username') !== 'admin') {
                this.items = [
                    {
                        icon: 'el-icon-school',
                        index: 'room',
                        title: '房间列表'
                    },
                    {
                        icon: 'el-icon-brush',
                        index: 'facilities',
                        title: '设施管理'
                    },
                    {
                        icon: 'el-icon-male',
                        index: 'sauna',
                        title: '男桑拿房'
                    },
                    {
                        icon: 'el-icon-female',
                        index: 'girl',
                        title: '女桑拿房'
                    },
                    {
                        icon: 'el-icon-cold-drink',
                        index: 'bar',
                        title: '酒吧走廊'
                    },
                    {
                        icon: 'el-icon-dish',
                        index: 'canteen',
                        title: '餐厅'
                    }
                ];
            } else {
                this.items = [
                    {
                        icon: 'el-icon-school',
                        index: 'room',
                        title: '房间列表'
                    },
                    {
                        icon: 'el-icon-brush',
                        index: 'facilities',
                        title: '设施管理'
                    },
                    {
                        icon: 'el-icon-s-custom',
                        index: 'staff',
                        title: '酒店员工信息'
                    },
                    {
                        icon: 'el-icon-male',
                        index: 'sauna',
                        title: '男桑拿房'
                    },
                    {
                        icon: 'el-icon-female',
                        index: 'girl',
                        title: '女桑拿房'
                    },
                    {
                        icon: 'el-icon-cold-drink',
                        index: 'bar',
                        title: '酒吧走廊'
                    },
                    {
                        icon: 'el-icon-dish',
                        index: 'canteen',
                        title: '餐厅'
                    },
                    {
                        icon: 'el-icon-lx-profile',
                        index: 'permission',
                        title: '前台管理'
                    }
                ];
            }
        }
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
    overflow-y: scroll;
}

.sidebar::-webkit-scrollbar {
    width: 0;
}

.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}

.sidebar > ul {
    height: 100%;
}
</style>
