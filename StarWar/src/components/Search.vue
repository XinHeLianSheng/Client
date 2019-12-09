<template>
  <div id="example-1">
    <div id="title">
      <p>
        <router-link to="/">登录</router-link>
      </p>
      <p>
        <router-link to="/Signup">注册</router-link>
      </p>
    </div>
    <div>
      <input id="search" placeholder="目的路径，如people/1" v-model="content"></input>
      <button v-on:click="get()">搜索</button>
    </div>
    <div id="con">
      <textarea v-model="msg" rows="30" cols="100"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data: function() {
    return {
      msg: "",
      content: "",
      pages: 1,
      contentTag: "",
      currentPage: 1
    };
  },
  methods: {
    get: function() {
      var flag = /^([a-z]{1,10}((\/[0-9]*)|(\/\?page\=\d)))?$/.test(this.content);
      if(!flag) {
        alert("输入错误！！");
        return;
      }
      //console.log("login:",$cookies.get("LogInUser"));
      this.$http.get("http://localhost:8080/api/" + this.content).then(
        function(res) {
          if(res.ok) {
            this.msg = JSON.stringify(res.data, null, 4);
          }
        }, function() {
          alert("错误！！");
        });

      if(this.content != ""){
        this.getPages();
      }
    },

    getPages: function() {
      this.contentTag = this.content.split("/")[0];
      this.$http.get("http://localhost:8080/api/" + this.contentTag + "/pages").then(
        function(res) {
          this.pages = Math.ceil(parseInt(res.data)/5);
          this.currentPage = this.content.indexOf("=") != -1? parseInt(this.content.split("=")[1]) : 1;
        }, function() {
          alert("找不到页！！！");
        });
    }
  }
}
</script>

<style>
#con {
  margin-top: 10px;
}
</style>