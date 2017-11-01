<template>
    <div>
        <el-form :model="regForm" :rules="rules" label-width="100px" ref="regForm">
            <el-form-item label="用户名" prop="username">
                <el-input v-model="regForm.username"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
                <el-input v-model="regForm.password" type="password"></el-input>
            </el-form-item>
            <el-form-item label="确认密码" prop="checkPassword">
                <el-input v-model="regForm.checkPassword" type="password"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('regForm')">注册</el-button>
                <el-button @click="resetForm('regForm')" >重置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
import api from '../axios.js'

export default {
    data(){
        //自定义验证规则
        let validatePass1 = (rule, value, callback) => {
            // 6-16位, 数字, 字母, 字符至少包含两种, 同时不能包含中文和空格
            let reg = /(?!^[0-9]+$)(?!^[A-z]+$)(?!^[^A-z0-9]+$)^[^\s\u4e00-\u9fa5]{6,16}$/;
            if(!reg.test(value)){
                callback(new Error('密码长度需6-16位，且包含字母和字符'));
            }else{
                callback();
            }
        };
        //验证密码是否重复
        let validatePass2 = (rule, value, callback) => {
            if(value !== this.regForm.password){
                callback(new Error('两次密码输入不一致'));
            }else{
                callback();
            }
        };

        return {
            regForm: {
                username: '',
                password: '',
                checkPassword: ''
            },
            rules: {
                username: [
                    { required: true, message: '用户名不能少', trigger: 'blur'},
                    { min: 6, max: 16, message: '用户名在6到16位之间', trigger: 'blur'}
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur'},
                    { validator: validatePass1, trigger: 'blur'}
                ],
                checkPassword: [
                    { required: true, message: '请再次输入密码', trigger: 'blur'},
                    { validator: validatePass2, trigger: 'blur'}
                ]
            }
        }
    },
    methods: {
        resetForm(formName){
            this.$refs[formName].resetFields();
        },
        submitForm(formName){
            this.$refs[formName].validate((valid) => {
                if(valid){ //验证通过
                    api.userRegister(this.regForm)
                        .then(({ data }) => { 
                            if(data.success){
                                this.$message({
                                    type: 'success',
                                    message: '注册成功'
                                });
                            }else{
                                this.$message({
                                    type: 'info',
                                    message: '用户名已经存在'
                                });
                            }
                        });
                }else{ //验证不通过
                    return false;
                }
            });
        }
    }
}
</script>

<style scoped>
    
</style>