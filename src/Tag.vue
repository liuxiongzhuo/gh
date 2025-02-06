<script setup>
import { ref } from 'vue'
const props = defineProps(['owner', 'repo'])
const items = ref([])
const pathname = ref(window.location.pathname)
fetch('http://ghapi.0031400.xyz/' + pathname.value)
    .then(res => res.json())
    .then(data => {
        if (data == null) {
            window.location.href = '/404'
        } else {
            items.value = data
        }
    })
function lastItem(path) {
    return path.split('/')[path.split('/').length - 1]
}
</script>

<template>
    <div id="container">

        <div id="main">
            <el-card><el-link :href="'/' + props.owner">
                    {{ props.owner }}
                </el-link><el-divider direction="vertical" />{{ props.repo }}</el-card>
            <el-card>

                <template v-for="item in items">
                    <el-link :href="'http://ghapi.0031400.xyz'+item" class="mx-1">
                        {{ lastItem(item) }}
                    </el-link>
                    <el-divider style="margin: 10px 0;" />
                </template>
            </el-card>
        </div>
    </div>
</template>
<style scoped>
#main {
    margin: 0 auto;
    max-width: 800px;
}
</style>