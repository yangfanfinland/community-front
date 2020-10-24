<template>
  <!-- 修改密码 -->
  <div class="layui-form layui-form-pane layui-tab-item layui-show">
    <form>
      <validation-observer ref="observer" v-slot="{ validate }">
        <div class="layui-form-item">
          <validation-provider name="oldpassword" rules="required|min:6|max:16" v-slot="{errors}">
            <div class="layui-row">
              <label for="L_nowpass" class="layui-form-label">Current password</label>
              <div class="layui-input-inline">
                <input type="password" v-model="oldpassword" class="layui-input" />
              </div>
            </div>
            <div class="layui-row">
              <span style="color: #c00;">{{errors[0]}}</span>
            </div>
          </validation-provider>
        </div>
        <div class="layui-form-item">
          <validation-provider
            name="password"
            rules="required|min:6|max:16|confirmed:confirmation"
            v-slot="{errors}"
          >
            <div class="layui-row">
              <label for="L_pass" class="layui-form-label">New password</label>
              <div class="layui-input-inline">
                <input type="password" v-model="password" class="layui-input" />
              </div>
              <div class="layui-form-mid layui-word-aux">6-16 characters</div>
            </div>
            <div class="layui-row">
              <span style="color: #c00;">{{errors[0]}}</span>
            </div>
          </validation-provider>
        </div>
        <div class="layui-form-item">
          <validation-provider v-slot="{ errors }" vid="confirmation">
            <div class="layui-row">
              <label for="L_repass" class="layui-form-label">Confirm password</label>
              <div class="layui-input-inline">
                <input type="password" v-model="repassword" class="layui-input" />
              </div>
            </div>
            <div class="layui-row">
              <span style="color: #c00;">{{errors[0]}}</span>
            </div>
          </validation-provider>
        </div>
        <div class="layui-form-item">
          <button type="button" class="layui-btn" @click="validate().then(submit)">Update</button>
        </div>
      </validation-observer>
    </form>
  </div>
</template>

<script>
import { ValidationProvider, ValidationObserver } from 'vee-validate'
import { changePasswd } from '@/api/user'
export default {
  name: 'password',
  components: {
    ValidationProvider,
    ValidationObserver
  },
  data () {
    return {
      oldpassword: '',
      password: '',
      repassword: ''
    }
  },
  methods: {
    async submit () {
      const isValid = await this.$refs.observer.validate()
      if (!isValid) {
        // ABORT!!
        return
      }
      if (this.oldpassword === this.password) {
        this.$alert('The new password should not same with old one!')
        return
      }
      changePasswd({
        oldpwd: this.oldpassword,
        newpwd: this.password
      }).then((res) => {
        if (res.code === 200) {
          this.$alert('Update password successfully!')
          this.oldpassword = ''
          this.password = ''
          this.repassword = ''
          requestAnimationFrame(() => {
            this.$refs.observer.reset()
          })
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.layui-form-pane {
  .layui-form-label {
    width: 150px;
    text-align: left;
  }
}
</style>
