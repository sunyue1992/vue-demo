<template>
    <div>
        <el-table
                :data="tableData"
                stripe
                style="width: 100%">

            <el-table-column
                    prop="password"
                    width="180"
                    type="expand"
                    label="地址">
                <template scope="props">
                    <el-form label-position="left" inline class="demo-table-expand">
                        <el-form-item label="商品名称">
                            <span>{{ props.row.username }}</span>
                        </el-form-item>
                        <el-form-item label="所属店铺">
                            <span>{{ props.row.username }}</span>
                        </el-form-item>
                    </el-form>
                </template>
            </el-table-column>

            <el-table-column
                    label="日期"
                    width="180"
                    prop="createdAt"
                    :formatter="formatterDate">
            </el-table-column>
            <el-table-column
                    prop="username"
                    label="姓名"
                    width="180">
            </el-table-column>

            <el-table-column
                    width="180"
                    label="自定义">
                <template scope="scope">
                    <el-popover trigger="hover" placement="top">
                        <p>姓名: {{ scope.row.username }}</p>
                        <div slot="reference" class="name-wrapper">
                            <el-tag>{{ scope.row.username }}</el-tag>
                        </div>
                    </el-popover>
                </template>
            </el-table-column>
        </el-table>

        <el-radio v-model="isDisable" label="禁用">禁用</el-radio>
        <el-radio v-model="isDisable" label="选中且禁用">选中且禁用</el-radio>
        <el-upload
                class="upload-demo"
                action="https://jsonplaceholder.typicode.com/posts/"
                :before-upload="handlePreview"
                :on-remove="handleRemove"
                :file-list="fileList">
            <el-button size="small" type="primary">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>

        <el-progress :text-inside="true" :stroke-width="18" :percentage="0"></el-progress>
        <el-progress :text-inside="false" type="circle" :stroke-width="18" :percentage="70"></el-progress>
        <el-progress :text-inside="true" :stroke-width="18" :percentage="100" status="success"></el-progress>
        <el-progress :text-inside="true" :stroke-width="18" :percentage="50" status="exception"></el-progress>
        <el-button
                type="primary"
                @click="openFullScreen"
                v-loading.fullscreen.lock="fullscreenLoading">
            显示整页加载，3 秒后消失



        </el-button>
    </div>

</template>

<script>
    import {
        mapGetters,
        mapActions
    } from 'vuex'
    import moment from 'moment'
    import {formatterDate} from '../../store/util'
    export default {
        created(){
            this.getTableDisplayData()
        },
        computed: {
            ...mapGetters('table/',{
                tableData: 'tableDisplayData'
            }),
            isDisable: {
                get(){
                    return this.$store.state.table.isDisable
                },
                set(isDisable){
                    this.$store.dispatch('table/setDisable',isDisable)
                }
            }
        },
        methods: {

            ...mapActions('table/',{
                getTableDisplayData: 'getTableDisplayData'
            }),
            formatterDate,
            handleRemove(file,fileList) {
                console.log(file,fileList)
            },
            handlePreview(file) {
                console.log(file)
            },
            openFullScreen() {
                this.fullscreenLoading = true
                setTimeout(() => {
                    this.fullscreenLoading = false
                },1000)
            }
        },
        data() {
            return {
                fullscreenLoading: false,
                fileList: [{
                    name: 'food.jpeg',
                    url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'
                },{
                    name: 'food2.jpeg',
                    url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'
                }]
            }
        }
    }
</script>