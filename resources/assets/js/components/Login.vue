<template>
    <div>
        <div class="alert alert-danger" v-if="error">
            <a class="close" @click="hideAlert">&times;</a>
            <p>{{ remoteMsg }}</p>
        </div>
        <form autocomplete="off" @submit.prevent="login" method="post">
            <div class="form-group">
                <label for="email">E-mail</label>
                <input type="email" id="email" class="form-control" placeholder="user@example.com" v-model="email" required>
            </div>
            <div class="form-group">
                <label for="password">Password</label>
                <input type="password" id="password" class="form-control" v-model="password" required>
            </div>
            <button type="submit" class="btn btn-default">Sign in</button>
        </form>
    </div>
</template>

<script>
  export default {
    data(){
      return {
        email: null,
        password: null,
        error: false,
        remoteMsg: ''
      }
    },
    methods: {
      login(){
        var app = this
        this.$auth.login({
            params: {
              email: app.email,
              password: app.password
            }, 
            success: function () {},
            error: function (e) {
                const errResp = e.response.data;
                app.error = true;
                app.remoteMsg = errResp.msg;
                console.log('[Error] Login: ' + errResp.msg);
            },
            rememberMe: true,
            redirect: '/dashboard',
            fetchUser: true,
        });       
      },
      hideAlert() {
          this.error = false;
      }
    }
  } 
</script>