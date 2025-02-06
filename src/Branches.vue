<script setup>
import { ref } from 'vue'
const props = defineProps(['owner', 'repo'])
const branches = ref([])
fetch('http://ghapi.0031400.xyz/' + props.owner + '/' + props.repo + '/branches')
    .then(res => res.json())
    .then(data => {
        if (data == null) {
            window.location.href = '/404'
        } else {
            branches.value = data
        }
    })
</script>

<template>
    <div id="container">

        <div id="main">
            <el-card><el-link :href="'/' + props.owner">
                    {{ props.owner }}
                </el-link><el-divider direction="vertical" />{{ props.repo }}</el-card>
            <el-card>

                <template v-for="branch in branches">
                    <el-link :href="'/' + props.owner + '/' + props.repo +'/tree/'+branch"
                        class="mx-1">
                        {{ branch }}
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