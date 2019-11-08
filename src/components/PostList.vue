<template>
  <div class="postList">
    <!-- 在数据未返回的时候，显示这个正在加载的GIF -->
    <div class="loading" v-if="isLoading"></div>
    <!-- 代表我们的主题代表 -->
    <div class="posts" v-else>
      <ul>
        <li>
          <div class="toobar">
            <span>全部</span>
            <span>精华</span>
            <span>分享</span>
            <span>问答</span>
            <span>招聘</span>
          </div>
        </li>
        <li v-for="post in posts">
          <!-- 头像 -->
          <img :src="post.author.avatar_url" alt />
          <!-- 回复/浏览 -->
          <span class="allcount">
            <span class="reply_count">{{post.reply_count}}/{{post.visit_count}}</span>
          </span>
          <!-- 帖子分类 -->
          <span
            :class="[{out_good:(post.good == true),put_top:(post.top == true),topiclistTab:(posts.good != true && post.top != true)}]"
          >{{post | tabFormatter}}</span>
          <!-- 标题 -->
          <router-link :to="{name:'post_content',params:{id:post.id}}">
            <span>{{post.title}}</span>
          </router-link>

          <!-- 发帖时间 -->
          <span class="lastReply">{{post.last_reply_at | formatDate}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "PostList",
  data: function() {
    return {
      isLoading: false,
      //页面列表数组
      posts: []
    };
  },
  beforeMount: function() {
    this.isLoading = true; //加载成功前显示加载动画
    this.getData(); //在页面加载之前获取数据
  },
  methods: {
    getData: function() {
      this.$axios
        .get("https://cnodejs.org/api/v1/topics", {
          params: {
            page: 1,
            limit: 20
          }
        })
        .then(response => {
          console.log(response);
          this.isLoading = false; //加载完成
          this.posts = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style scoped>
.postList {
  background-color: #e1e1e1;
}
.posts {
  margin-top: 10px;
}
.postList img {
  width: 30px;
  width: 30px;
  vertical-align: middle;
}
ul {
  list-style: none;
  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
}
ul li:not(:first-child) {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma,
    "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
}
li:not(:first-child):hover {
  background: white;
}
li span {
  line-height: 30px;
}
.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
}
.reply_count {
  color: #9e78c0;
  font-size: 14px;
}
.put_good,
.put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}
.topiclistTab {
  background-color: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}
.lastReply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}
.toobar {
  height: 40px;
  background-color: #f5f5f5;
}
.toobar span {
  font-size: 14px;
  color: #80bd01;
  line-height: 40px;
  margin: 0 10px;
  cursor: pointer;
}
.toobar span:hover {
  color: #9e78c0;
}
</style>

