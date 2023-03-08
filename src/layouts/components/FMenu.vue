<template>
    <div class="f-menu" :style="{ width:$store.state.asideWidth }">
        <el-menu :default-active="defaultActive" unique-opened :collapse="isCollapse" default-active="2" class="border-0" @select="handleSelect" :collapse-transition="false">

            <!-- 循环遍历菜单栏数据 -->
            <template v-for="(item,index) in asideMenus" :key="index">
                <!-- 当数据有子节点的时候，继续遍历菜单 -->
                <el-sub-menu v-if="item.child && item.child.length > 0" :index="item.name">
                    <!-- title模板 设置菜单名称和icon-->
                    <template #title>
                        <el-icon>
                            <component :is="item.icon"></component>
                        </el-icon>
                        <span>{{ item.name }}</span>
                    </template>
                    <!-- 循环遍历二级菜单 -->
                    <!-- 按钮 -->
                    <el-menu-item v-for="(item2,index2) in item.child" :key="index2" :index="item2.frontpath">
                        <el-icon>
                            <component :is="item2.icon"></component>
                        </el-icon>
                        <span>{{ item2.name }}</span>
                    </el-menu-item>
                </el-sub-menu>
                <!-- 当菜单中没有子节点时，直接渲染菜单 -->
                <el-menu-item v-else :index="item.frontpath">
                    <el-icon>
                         <component :is="item.icon"></component>
                    </el-icon>
                    <span>{{ item.name }}</span>
                </el-menu-item>
            </template>
        </el-menu>
    </div>
</template>
<script setup>
import { computed,ref } from 'vue';
import { useRouter,useRoute,onBeforeRouteUpdate } from 'vue-router';
import { useStore } from 'vuex';
const router = useRouter()
const store = useStore()
const route = useRoute()

// 默认选中
const defaultActive = ref(route.path)

// 监听路由变化
onBeforeRouteUpdate((to,from)=>{
    defaultActive.value = to.path
})

// 是否折叠
const isCollapse = computed(()=> !(store.state.asideWidth == '250px'))

const asideMenus = computed(()=>store.state.menus.slice(0,5))

const handleSelect = (e)=>{
    router.push(e)
}
</script>
<style>
.f-menu {
    transition: all 0.5s;
    top: 64px;
    bottom: 0;
    left: 0;
    overflow-y: auto;
    overflow-x: hidden;
    @apply shadow-md fixed bg-light-50;
}
.f-menu::-webkit-scrollbar{
    width: 0px;
}
</style>