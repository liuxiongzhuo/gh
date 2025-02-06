<script setup>
import { ref } from 'vue'
const props = defineProps(['owner', 'repo', 'branch'])
const items = ref([])
let path_array = window.location.pathname.split('/')
if (path_array[0]=='') {
  path_array.shift()
}
if (path_array[path_array.length-1]=='') {
  path_array.pop()
}
let backPath =''
if (path_array.length>4) {
    path_array.pop()
    backPath= path_array.join('/')
}

fetch('http://ghapi.0031400.xyz' + window.location.pathname)
    .then(async res => {
        if (res.headers.get('content-type') != 'application/json') {
            window.location.href = 'http://ghapi.0031400.xyz' + window.location.pathname
        } else {
            const data = await res.json()
            if (data == null) {
                window.location.href = '/404'
            } else {
                items.value = data
            }
        }
    })
</script>

<template>
    <div id="container">

        <div id="main">
            <el-card><el-link :href="'/'+props.owner">
                {{ props.owner }}
            </el-link><el-divider direction="vertical" /><el-link :href="'/' + props.owner + '/' + props.repo ">{{ props.repo }}</el-link><el-divider direction="vertical" /><el-link
            :href="'/' + props.owner + '/' + props.repo + '/branches'">{{ props.branch }}</el-link><el-divider direction="vertical" /><el-link
            :href="'/' + props.owner + '/' + props.repo + '/releases'">releases</el-link></el-card>
            <el-card>
                <template v-if="backPath!=''">

                    <el-icon>
                            <folder />
                        </el-icon>
                        <el-link :href="'/'+backPath"
                            class="mx-1">..</el-link><el-divider style="margin: 10px 0;" />
                </template>
                <template v-for="item in items">
                    <template v-if="item.contentType == 'file'">
                        <el-icon >
                            <document />
                        </el-icon>
                        <!-- <el-link :href="'http://ghapi.0031400.xyz/'+props.owner+'/'+props.repo+'/tree/'+props.branch+'/' + item.path" class="mx-1">{{ item.name }} -->
                        <el-link :href="'http://down.0031400.xyz/?url=https://raw.githubusercontent.com/'+props.owner+'/'+props.repo+'/refs/heads/'+props.branch+'/' + item.path" class="mx-1">{{ item.name }}
                        </el-link>
                    </template>
                    <template v-if="item.contentType == 'directory'">
                        <el-icon >
                            <folder />
                        </el-icon>
                        <el-link :href="'/' + props.owner + '/' + props.repo + '/tree/' + props.branch + '/' + item.path" class="mx-1">{{ item.name }}
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