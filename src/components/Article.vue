<template>
  <div class="article">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt />
    </div>
    <div v-else>
      <div class="topic_header">
        <div class="topic_title">{{post.title}}</div>
        <ul>
          <li>发布于：{{post.create_at | formatDate}}</li>
          <li>作者:{{post.loginname}}</li>
          <li>{{post.visit_count}}次浏览</li>
          <li>来自{{post | tabFormatter}}</li>
        </ul>
        <div v-html="post.content" class="topic_content"></div>
      </div>
      <div id="reply">
        <div class="topbar">回复</div>
        <div v-for="(reply,index) in post.replies" class="replySec">
          <div class="replyUp">
            <router-link :to="{
              name:'user_info',
              params: {
                name:reply.author.loginname
              }
            }">
              <img :src="reply.author.avatar_url" alt />
            </router-link>
            <router-link>
              <span>{{reply.author.loginname}}</span>
            </router-link>

            <span>{{index+1}}楼</span>
            <span v-if="reply.ups.length > 0">{{reply.ups.length}}</span>
            <span v-else></span>
          </div>
          <p v-html="reply.content"></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data: function() {
    return {
      isLoading: false,
      post: {} //代表当前文章页的所有内容，属性
    };
  },
  beforeMount: function() {
    this.isLoading = true;
    this.getArticleData();
  },
  methods: {
    getArticleData: function() {
      this.$axios
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then(response => {
          if (response.data.success == true) {
            this.isLoading = false;
            this.post = response.data.data;
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
@import url("../assets/markdown-github.css");
</style>