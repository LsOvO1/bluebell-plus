<template>
  <div class="main">
    <div class="container">
      <h2 class="form-title">欢迎注册</h2>
      <div class="form-group">
        <label for="name"><span class="required">*</span>用户名</label>
        <el-input type="text" required name="name" id="name" placeholder="请输入用户名" v-model="username" />
      </div>
      <div class="form-group">
        <label for="email"><span class="required">*</span>邮箱</label>
        <el-input type="email" required name="email" id="email" placeholder="请输入邮箱" v-model="email" />
      </div>
      <div class="form-group">
        <label for="pass"><span class="required">*</span>密码</label>
        <el-input type="password" required name="pass" id="pass" placeholder="请输入密码" v-model="password" />
      </div>
      <div class="form-group">
        <label for="re_pass"><span class="required">*</span>确认密码</label>
        <el-input type="password" required name="re_pass" id="re_pass" placeholder="请再次输入密码" v-model="confirm_password" />
      </div>
      <div class="form-group">
        <label for="gender"><span class="required">*</span>性别</label>
        <div id="gender" class="gender-group">
          <el-radio v-model="gender" :label="1">男</el-radio>
          <el-radio v-model="gender" :label="2">女</el-radio>
        </div>
      </div>
      <div class="form-btn">
        <button type="button" class="btn btn-info" @click="submit">注册</button>
      </div>
      <div class="form-footer">
        <router-link to="/login" class="login-link">已有账号？立即登录</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SignUp",
  data() {
    return {
      username: "",
      password: "",
      email: '',
      gender: 1,
      confirm_password: "",
      submitted: false
    };
  },
  methods: {
    submit() {
      if (!this.username || !this.email || !this.password || !this.confirm_password) {
        this.$message.warning('请填写完整信息');
        return;
      }
      if (this.password !== this.confirm_password) {
        this.$message.error('两次输入的密码不一致');
        return;
      }
      this.$axios({
        method: 'post',
        url: '/signup',
        data: {
          username: this.username,
          email: this.email,
          gender: this.gender,
          password: this.password,
          confirm_password: this.confirm_password
        }
      }).then((res) => {
        if (res.code == 1000) {
          this.$message.success('注册成功，请登录');
          this.$router.push({ name: "Login" });
        } else {
          this.$message.error(res.msg || '注册失败');
        }
      }).catch(() => {
        this.$message.error('注册失败，请稍后重试');
      })
    }
  }
};
</script>

<style lang="less" scoped>
.main {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding: 100px 0;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;

  .container {
    width: 400px;
    background: rgba(255, 255, 255, 0.95);
    margin: 0 auto;
    padding: 40px;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(10px);

    .form-title {
      margin-bottom: 30px;
      text-align: center;
      font-size: 24px;
      color: #333;
      font-weight: 600;
    }

    .form-group {
      margin-bottom: 20px;

      label {
        display: block;
        margin-bottom: 8px;
        font-weight: 500;
        color: #333;

        .required {
          color: #ff4d4f;
          margin-right: 4px;
        }
      }

      :deep(.el-input__inner) {
        height: 40px;
        line-height: 40px;
        border-radius: 4px;
        border: 1px solid #ddd;
        transition: all 0.3s;

        &:focus {
          border-color: #1890ff;
          box-shadow: 0 0 0 2px rgba(24, 144, 255, 0.2);
        }
      }

      .gender-group {
        display: flex;
        gap: 20px;
      }
    }

    .form-btn {
      margin-top: 30px;
      text-align: center;

      .btn {
        width: 100%;
        height: 40px;
        font-size: 16px;
        font-weight: 500;
        color: #fff;
        background: linear-gradient(45deg, #1890ff, #36cfc9);
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: all 0.3s;

        &:hover {
          transform: translateY(-2px);
          box-shadow: 0 4px 12px rgba(24, 144, 255, 0.3);
        }
      }
    }

    .form-footer {
      margin-top: 20px;
      text-align: center;

      .login-link {
        color: #1890ff;
        text-decoration: none;
        font-size: 14px;
        transition: all 0.3s;

        &:hover {
          color: #40a9ff;
          text-decoration: underline;
        }
      }
    }
  }
}
</style>