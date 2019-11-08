<template>
  <div class="UserInfo">
    <!-- 在数据未返回的时候，显示这个正在加载的GIF -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt />
    </div>
    <div class="userInfomation" v-else>
      <section>
        <img :src="userinfo.avatar_url" alt="">
        <span>{{userinfo.loginname}}</span>
        <p>
          {{userinfo.score}}积分
        </p>
        <p>
          注册时间{{userinfo.create_at | formatDate}}
        </p>
      </section>
      <div class="replies">
        <p>回复的主题</p>
        <ul>
          <li v-for="item in items">
            <router-link :to="{
              name:'post_content',
              params: {
                id:item.id
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
      <div class="topics">
        <p>创建的主题</p>
        <ul>
          <li v-for="item in userinfo.recent_topics">
            <router-link :to="{
              name:'post_content',
              params: {
                id:item.id
              }
            }">
              {{item.title}}
            </router-link>
          </li>  
        </ul>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "UserInfo",
  data: function() {
    return {
      isLoading: false,
      userinfo: {},
      items:[]
    };
  },
  beforeMount:function(){
    this.getData()
    this.isLoading = true
  },
  methods: {
    getData: function() {
      this.$axios
        .get(` https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(response => {
          if (response.data.success == true) {
            this.isLoading = false;
            this.userinfo = response.data.data;
            this.items = this.userinfo.recent_replies
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style>
</style>