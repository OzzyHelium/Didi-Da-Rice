<template>
    <el-dialog style="width: 500px" align-center>
        <div class="descriptions">
            <el-descriptions title="确认信息" :column=1 border>
                <el-descriptions-item label="姓名">这是名字</el-descriptions-item>
                <el-descriptions-item label="电话号码">这是手机号</el-descriptions-item>
                <el-descriptions-item label="校区">{{ props.form["area"] }}</el-descriptions-item>
                <el-descriptions-item label="取餐地点">{{ props.form["fromAddress"] }}</el-descriptions-item>
                <el-descriptions-item label="送餐地点">{{ props.form["toAddress"] }}</el-descriptions-item>
                <el-descriptions-item label="已送达" v-if="props.form['delivery']">是</el-descriptions-item>
                <el-descriptions-item label="已送达" v-if="!props.form['delivery']">否</el-descriptions-item>
                <el-descriptions-item label="送达时间" v-if="!props.form['delivery']">{{ props.form["date1"]+'  '+props.form["date2"] }}</el-descriptions-item>
                <el-descriptions-item label="备注" v-if="props.form['desc']!=''">{{ props.form["desc"] }}</el-descriptions-item>
                <el-descriptions-item label="备注" v-else>无</el-descriptions-item>
            </el-descriptions>
        </div>
        <div style="display: flex;">
            <div class="flex-grow" />
            <span class="dialog-footer">
                <el-button type="primary" @click="$emit('emitOrder')">
                    Confirm
                </el-button>
                <el-button @click="$emit('cancelDialog')">Cancel</el-button>
            </span>
        </div>
    </el-dialog>
</template>
<script lang="ts" setup>
const props = defineProps(['form'])
defineEmits(["cancelDialog","emitOrder"])
</script>
<style scoped>
.dialog-footer button:first-child {
    margin-right: 10px;
}

.descriptions {
    width: 330px;
    height: 330px;
    margin: 30px auto 30px auto;
}

.flex-grow {
    flex-grow: 1;
}
</style>