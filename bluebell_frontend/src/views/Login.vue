<template>
  <div class="main">
    <div class="container">
      <h2 class="form-title">欢迎登录</h2>
      <div class="form-group">
        <label for="name">用户名</label>
        <input type="text" class="form-control" v-model="username" name="name" id="name" placeholder="请输入用户名" />
      </div>
      <div class="form-group">
        <label for="pass">密码</label>
        <input type="password" class="form-control" v-model="password" name="pass" id="pass" placeholder="请输入密码" />
      </div>
      <div class="form-btn">
        <button type="button" class="btn btn-info" @click="submit">登录</button>
      </div>
      <div class="form-footer">
        <router-link to="/signup" class="signup-link">还没有账号？立即注册</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
      submitted: false
    };
  },
  computed: {
  },
  created() {

  },
  methods: {
    submit() {
      this.$axios({
        method: 'post',
        url: '/login',
        data: JSON.stringify({
          username: this.username,
          password: this.password
        })
      }).then((res) => {
        if (res.code == 1000) {
          localStorage.setItem("loginResult", JSON.stringify(res.data));
          this.$store.commit("login", res.data);
          this.$router.push({ path: this.redirect || '/' })
        } else {
          this.$message.error(res.msg || '登录失败')
        }
      }).catch(() => {
        this.$message.error('登录失败，请稍后重试')
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
      }

      .form-control {
        display: block;
        width: 100%;
        height: 40px;
        padding: 8px 12px;
        font-size: 14px;
        line-height: 1.5;
        color: #333;
        background-color: #fff;
        border: 1px solid #ddd;
        border-radius: 4px;
        transition: all 0.3s;

        &:focus {
          border-color: #1890ff;
          box-shadow: 0 0 0 2px rgba(24, 144, 255, 0.2);
          outline: none;
        }

        &::placeholder {
          color: #999;
        }
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

      .signup-link {
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