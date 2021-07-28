<template>
    <div class="nav_re">
        <el-menu
                id="menu"
                :default-active="activeIndex2"
                class="el-menu-demo"
                mode="horizontal"
                @select="handleSelect"
                background-color="#545c64"
                text-color="#fff"
                active-text-color="#ffd04b"
        >
            <el-menu-item index="1" class="item">处理中心</el-menu-item>
            <el-submenu index="2" class="item">
                <template slot="title" >我的工作台</template>
                <el-menu-item index="2-1">选项1</el-menu-item>
                <el-menu-item index="2-2">选项2</el-menu-item>
                <el-menu-item index="2-3">选项3</el-menu-item>
            </el-submenu>
            <el-menu-item index="3" disabled class="item">消息中心</el-menu-item>
            <el-menu-item index="4" class="item">订单管理</el-menu-item>
        </el-menu>
    </div>
</template>

<script>
    export default {
        name: "travelHeader",
        data() {
            return {
                activeIndex: '1',
                activeIndex2: '1'
            };
        },
        methods: {
            handleSelect(key, keyPath) {
                console.log(key, keyPath);
            }
        }
    }
</script>

<style>
.nav_re{
    width: 100%;

}
#menu{
 padding-left: 200px;

}

.item{
    display: block;

}
</style>