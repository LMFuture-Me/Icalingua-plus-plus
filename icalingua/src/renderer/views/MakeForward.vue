<template>
    <div class="settings">
        <p>合并转发 DEBUG</p>
        <p style="font-weight:bold; color:red;">警告：本页面功能只用于 DEBUG，使用本页面功能造成的后果自行承担</p>
        <el-input v-model="uid" placeholder="uid" @input="uid = uid.replace(/[^\d]/g,'')"/>
        <el-input v-model="nickname" placeholder="nickname"/>
        <el-input v-model="content" type="textarea" placeholder="content" :rows="6"/>
        <el-input v-model="timestamp" placeholder="timestamp" @input="timestamp = timestamp.replace(/[^\d]/g,'')" />
        <div class="dialog-footer">
            <el-button type="primary" @click="addMessage">加入</el-button>
            <el-button type="primary" @click="createForward">生成</el-button>
        </div>
    </div>
</template>

<script>
import ipc from '../utils/ipc'

export default {
    name: 'MakeForward',
    data() {
        return {
            uid: '',
            nickname: '',
            content: '',
            timestamp: '',
            Messages: [],
        }
    },
    created() {
        document.title = '合并转发 DEBUG'
    },
    methods: {
        addMessage() {
            const singleMessage = {
                user_id: 0,
                message: '',
                nickname: '',
                time: 0,
            }
            if (this.uid != '') {
                if(this.timestamp == '') {
                    this.timestamp = Math.floor(new Date().getTime() / 1000).toString()
                }
                if(this.nickname == '') {
                    this.nickname = this.uid
                }
                singleMessage.user_id = parseInt(this.uid)
                singleMessage.message = this.content
                singleMessage.nickname = this.nickname
                singleMessage.time = parseInt(this.timestamp)
                this.Messages.push(singleMessage)
                this.uid = ''
                this.nickname = ''
                this.content = ''
                this.timestamp = ''
            }
            console.log(this.Messages)
        },
        createForward() {
            if (this.Messages.length > 0) {
                ipc.makeForward(this.Messages)
            }
            window.close()
        },
    },
}
</script>

<style scoped>
.settings {
    padding: 0 16px;
}

.dialog-footer {
    margin-top: 14px;
    text-align: center;
}
</style>
