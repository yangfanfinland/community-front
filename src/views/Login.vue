<template>
  <div class="layui-container fly-marginTop">
    <div class="fly-panel fly-panel-user" pad20>
      <div class="layui-tab layui-tab-brief" lay-filter="user">
        <ul class="layui-tab-title">
          <li class="layui-this">Login</li>
          <li>
            <router-link :to="{name: 'reg'}">Register</router-link>
          </li>
        </ul>
        <div class="layui-form layui-tab-content" id="LAY_ucm" style="padding: 20px 0;">
          <validation-observer ref="observer" v-slot="{ validate }">
            <div class="layui-tab-item layui-show">
              <div class="layui-form layui-form-pane">
                <form method="post">
                  <div class="layui-form-item">
                    <label for="L_email" class="layui-form-label">Email</label>
                    <validation-provider name="email" rules="required|email" v-slot="{errors}">
                      <div class="layui-input-inline">
                        <input
                          type="text"
                          name="username"
                          v-model="username"
                          placeholder="Please fill in email"
                          autocomplete="off"
                          class="layui-input"
                        />
                      </div>
                      <div class="layui-form-mid">
                        <span style="color: #c00;">{{errors[0]}}</span>
                      </div>
                    </validation-provider>
                  </div>
                  <div class="layui-form-item">
                    <label for="L_pass" class="layui-form-label">Password</label>
                    <validation-provider name="password" rules="required|min:6" v-slot="{errors}">
                      <div class="layui-input-inline">
                        <input
                          type="password"
                          name="password"
                          v-model="password"
                          placeholder="Please fill in password"
                          autocomplete="off"
                          class="layui-input"
                        />
                      </div>
                      <div class="layui-form-mid">
                        <span style="color: #c00;">{{errors[0]}}</span>
                      </div>
                    </validation-provider>
                  </div>
                  <div class="layui-form-item">
                    <validation-provider
                      name="code"
                      ref="codefield"
                      rules="required|length:4"
                      v-slot="{errors}"
                    >
                      <div class="layui-row">
                        <label for="L_vercode" class="layui-form-label">Verification code</label>
                        <div class="layui-input-inline">
                          <input
                            type="text"
                            name="code"
                            v-model="code"
                            placeholder="Please fill in verification code"
                            autocomplete="off"
                            class="layui-input"
                          />
                        </div>
                        <div class>
                          <span class="svg" style="color: #c00;" @click="_getCode()" v-html="svg"></span>
                        </div>
                      </div>
                      <div class="layui-form-mid">
                        <span style="color: #c00;">{{errors[0]}}</span>
                      </div>
                    </validation-provider>
                  </div>
                  <div class="layui-form-item">
                    <button class="layui-btn" type="button" @click="validate().then(submit)">Login</button>
                    <span style="padding-left:20px;">
                      <router-link :to="{name: 'forget'}">Forgot password?</router-link>
                    </span>
                  </div>
                </form>
              </div>
            </div>
          </validation-observer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ValidationProvider, ValidationObserver } from 'vee-validate'
import { getCode, login } from '@/api/login'
import uuid from 'uuid/v4'
export default {
  name: 'login',
  components: {
    ValidationProvider,
    ValidationObserver
  },
  data () {
    return {
      username: '',
      password: '',
      code: '',
      svg: ''
    }
  },
  mounted () {
    let sid = ''
    if (localStorage.getItem('sid')) {
      sid = localStorage.getItem('sid')
    } else {
      sid = uuid()
      localStorage.setItem('sid', sid)
    }
    this.$store.commit('setSid', sid)
    this._getCode()
  },
  methods: {
    _getCode () {
      let sid = this.$store.state.sid
      getCode(sid).then((res) => {
        if (res.code === 200) {
          this.svg = res.data
        }
      })
    },
    async submit () {
      const isValid = await this.$refs.observer.validate()
      if (!isValid) {
        // ABORT!!
        return
      }
      login({
        username: this.username,
        password: this.password,
        code: this.code,
        sid: this.$store.state.sid
      })
        .then((res) => {
          if (res.code === 200) {
            // 存储用户的登录名
            res.data.username = this.username
            this.$store.commit('setUserInfo', res.data)
            this.$store.commit('setIsLogin', true)
            this.$store.commit('setToken', res.token)
            this.username = ''
            this.password = ''
            this.code = ''
            requestAnimationFrame(() => {
              this.$refs.observer && this.$refs.observer.reset()
            })
            this.$router.push({ name: 'index' })
          } else if (res.code !== 200) {
            this.$refs.codefield.setErrors([res.msg])
          }
        })
        .catch((err) => {
          const data = err.response.data
          if (data.code === 500) {
            this.$alert('Email or password failed!')
          } else {
            this.$alert('Server error')
          }
        })
    }
  }
}
</script>

<style lang="scss" scoped>
// 公用样式可以放在App.vue中
.layui-form-label {
  width: 140px;
  text-align: left;
}
.layui-form-item {
  .layui-input-inline {
    width: 200px;
  }
}
</style>
