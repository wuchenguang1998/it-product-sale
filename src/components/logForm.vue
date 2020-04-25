<template>
  <div class="login-form">
    <div class="g-form">
      <div class="g-form-line">
        <span class="g-form-label">邮箱：</span>
        <div class="g-form-input">
          <input type="text" 
          v-model="usernameModel" placeholder="请输入用户名">
        </div>
        <span class="g-form-error">{{ userErrors.errorText }}</span>
      </div>
      <div class="g-form-line">
        <span class="g-form-label">密码：</span>
        <div class="g-form-input">
          <input type="password" 
          v-model="passwordModel" placeholder="请输入密码">
        </div>
        <span class="g-form-error">{{ passwordErrors.errorText }}</span>
      </div>
      <div class="g-form-line">
        <div class="g-form-btn">
          <a class="button" @click="onLogin">登录</a>
        </div>
      </div>
      <p>{{ errorText }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      usernameModel: '',
      passwordModel: '',
      errorText:''
    }
  },
  computed:{
    userErrors(){
      let errorText,status
      if(!/@/g.test(this.usernameModel)&&this.usernameModel.length>0){
        status=false
        if(this.usernameModel.length>0){
          errorText='请输入正确的邮箱名'
        }
      }else{
        status=true
        errorText=''
      }
      return{
        status,
        errorText
      }
    },
    passwordErrors(){
      let errorText,status
      if(!/^\w{1,6}$/g.test(this.passwordModel)){
        status=false
        if(this.passwordModel.length>0){
          errorText='请输入1-6位密码'
        }
      }else{
        status=true
        errorText=''
      }
      return{
        status,
        errorText
      }
    }
  },
  methods: {
    onLogin () {
      if(!this.userErrors.status||!this.passwordErrors.status){
        this.errorText='请输入正确的登录信息'
      }else{
        this.errorText=''
        this.$http.get('api/login')
        .then((res)=>{
          this.$emit('has-log',res.data)
        },(err)=>{
          console.log(err)
        })
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>