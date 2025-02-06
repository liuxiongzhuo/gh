<script setup>
import { ref } from 'vue'
const props = defineProps(['owner', 'repo'])
const tags = ref([])
fetch('http://ghapi.0031400.xyz/' + props.owner + '/' + props.repo + '/releases')
    .then(res => res.json())
    .then(data => {
        if (data == null) {
            window.location.href = '/404'
        } else {
            tags.value = data
        }
    })
</script>

<template>
    <div id="container">

        <div id="main">
            <el-card><el-link :href="'/' + props.owner">
                    {{ props.owner }}
                </el-link><el-divider direction="vertical" /><el-link :href="'/' + props.owner + '/' + props.repo ">{{ props.repo }}</el-link></el-card>
            <el-card>

                <template v-for="tag in tags">
                    <el-link :href="'/' + props.owner + '/' + props.repo +'/releases/tag/v'+tag"
                        class="mx-1">
                        {{ tag }}
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