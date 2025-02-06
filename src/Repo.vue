<script setup>
import { ref } from 'vue'
const props = defineProps(['owner', 'repo'])
const defaultBranch = ref()
const items = ref([])
fetch('http://ghapi.0031400.xyz/' + props.owner + '/' + props.repo)
    .then(res => res.json())
    .then(data => {
        if (data == null) {
            window.location.href = '/404'
        } else {
            items.value = data.items
            defaultBranch.value = data.defaultBranch
        }
    })
</script>

<template>
    <div id="container">
        <div id="main">
            <el-card>
                <el-link :href="'/' + props.owner">
                    {{ props.owner }}
                </el-link><el-divider direction="vertical" />{{ props.repo }}<el-divider direction="vertical" /><el-link
                    :href="'/' + props.owner + '/' + props.repo + '/branches'">{{
                        defaultBranch }}

                </el-link><el-divider direction="vertical" /><el-link
                    :href="'/' + props.owner + '/' + props.repo + '/releases'">releases</el-link>
            </el-card>
            <el-card>

                <template v-for="item in items">
                    <template v-if="item.contentType == 'file'">
                        <el-icon >
                            <document />
                        </el-icon>
                        <el-link :href="'http://down.0031400.xyz/?url=https://raw.githubusercontent.com/'+props.owner+'/'+props.repo+'/refs/heads/'+defaultBranch+'/' + item.path" class="mx-1">{{ item.name }}
                        </el-link>
                    </template>
                    <template v-if="item.contentType == 'directory'">
                        <el-icon >
                            <folder />
                        </el-icon>
                        <el-link :href="'/' + props.owner + '/' + props.repo + '/tree/' + defaultBranch + '/' + item.path" class="mx-1">{{ item.name }}
                        </el-link>
                    </template>
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