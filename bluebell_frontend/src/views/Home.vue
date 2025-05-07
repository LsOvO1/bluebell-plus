<template>
  <div>
    <div class="content">
      <div class="left">
        <SideBar></SideBar>
      </div>
      <div class="center">
        <!-- <h4 class="c-l-title">热门帖子</h4> -->
        <div class="c-l-header">
          <div class="new btn-iconfont" :class="{ active: timeOrder }" @click="selectOrder('time')">
            <i class="iconfont icon-polygonred"></i>最新
          </div>
          <div class="top btn-iconfont" :class="{ active: scoreOrder }" @click="selectOrder('score')">
            <i class="iconfont icon-top"></i>热门
          </div>
          <div class="search">
            <i class="search-icon el-icon-search" @click="searchPost"></i>
            <input type="text" class="s-input" v-model="keyword" @keydown.enter="searchPost" placeholder="搜索文章..." />
          </div>
          <div class="publish-btn" @click="goPublish">发表</div>
        </div>
        <ul class="c-l-list">
          <li class="c-l-item" v-for="post in postList" :key="post.post_id">
            <div class="post">
              <a class="vote">
                <span class="iconfont icon-up" @click="vote(post.post_id, 1)"></span>
              </a>
              <span class="text">{{ post.vote_num }}</span>
              <a class="vote">
                <span class="iconfont icon-down" @click="vote(post.post_id, -1)"></span>
              </a>
            </div>
            <div class="l-container" @click="goDetail(post.post_id)">
              <h4 class="con-title">{{ post.title }}</h4>
              <div class="con-memo">
                <p>{{ post.content }}</p>
              </div>
              <!-- <div class="user-btn">
              <span class="btn-item">
                <i class="iconfont icon-comment"></i>
                <span>{{post.comments}} comments</span>
              </span>
            </div> -->
            </div>
          </li>
          <div class="pagination-block">
            <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="1"
              :page-sizes="[5, 10, 20, 30]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper"
              :total="pageTotal.total">
            </el-pagination>
          </div>
        </ul>
      </div>
      <div class="right">
        <div class="run-time-container">
          <TimeMeter></TimeMeter>
        </div>
        <div class="github-project-card-container">
          <GithubProjectCard language="all"></GithubProjectCard>
        </div>
        <div class="github-golang-project-card-container">
          <GithubProjectCard language="golang" title="Golang热门项目排行榜"></GithubProjectCard>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from '../components/SideBar.vue';
// @ is an alias to /src
import TimeMeter from '../components/TimeMeter.vue';
import GithubProjectCard from './components/GithubProjectCard.vue';
import Vue from 'vue';
export default {
  name: "Home",
  components: { TimeMeter, SideBar, GithubProjectCard },
  data() {
    return {
      order: "time",
      postList: [],
      pageNumber: 1,
      pageSize: 5,
      pageTotal: {},
      keyword: '',
      isSearch: false
    };
  },
  methods: {
    selectOrder(order) {
      this.order = order;
      this.getPostList();
    },
    handleCurrentChange(val) {
      this.pageNumber = val;
      if (!this.isSearch) {
        this.getPostList();
      } else {
        this.searchPost();
      }
    },
    handleSizeChange(val) {
      this.pageSize = val;
      if (!this.isSearch) {
        this.getPostList();
      } else {
        this.searchPost();
      }
    },
    goDetail(id) {
      this.$router.push({ name: "Content", params: { id: id } });
    },
    getPostList() {
      this.$axios({
        method: "get",
        url: "/posts2",
        params: {
          page: this.pageNumber,
          size: this.pageSize,
          order: this.order,
        }
      })
        .then(response => {
          console.log(response.data, 222);
          if (response.code == 1000) {
            this.postList = response.data.list;
            this.pageTotal = response.data.page;
          } else {
            console.log(response.msg);
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    goPublish() {
      this.$router.push({ name: "Publish" });
    },
    vote(post_id, direction) {
      this.$axios({
        method: "post",
        url: "/vote",
        data: {
          post_id: post_id,
          direction: direction,
        }
      })
        .then(response => {
          if (response.code == 1000) {
            console.log("vote success");
            this.getPostList();
          } else if (response.code == 1009) {
            Vue.prototype.$message.error('请勿重复投票')
          } else if (response.code == 1010) {
            Vue.prototype.$message.error('已过投票时间')
          } else {
            console.log(response.msg);
            Vue.prototype.$message.error('请先登录')
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    async searchPost() {
      if (!this.keyword) {
        this.isSearch = false;
        this.getPostList();
        return;
      }
      this.isSearch = true;
      const response = await this.$axios({
        method: "get",
        url: "/search",
        params: {
          page: this.pageNumber,
          size: this.pageSize,
          search: this.keyword
        }
      });
      if (response.code === 1000) {
        console.log(response.data);
        this.postList = response.data.list;
        this.pageTotal = response.data.page;
      } else {
        console.log(response.message);
      }
    },
  },
  mounted: function () {
    this.getPostList();
  },
  computed: {
    timeOrder() {
      return this.order == "time";
    },
    scoreOrder() {
      return this.order == "score";
    }
  }
};
</script>

<style lang="less" scoped>
.content {
  display: flex;
  justify-content: space-between;
  margin-top: 48px;
  padding: 20px;
  min-height: calc(100vh - 48px);
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

.left {
  width: 300px;
  margin-right: 20px;
}

.center {
  flex: 1;
  background-color: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  backdrop-filter: blur(10px);
}

.right {
  width: 300px;
  margin-left: 20px;
}

.c-l-header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 2px solid #e8e8e8;
}

.btn-iconfont {
  display: flex;
  align-items: center;
  padding: 8px 16px;
  margin-right: 10px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
  
  i {
    margin-right: 5px;
  }
  
  &.active {
    background-color: #1890ff;
    color: #fff;
  }
  
  &:hover {
    background-color: #e6f7ff;
    color: #1890ff;
  }
}

.search {
  position: relative;
  flex: 1;
  margin: 0 20px;
  z-index: 1;
  
  .search-icon {
    position: absolute;
    left: 10px;
    top: 50%;
    transform: translateY(-50%);
    color: #999;
    cursor: pointer;
    transition: color 0.3s;
    
    &:hover {
      color: #1890ff;
    }
  }
  
  .s-input {
    width: 100%;
    height: 36px;
    padding: 0 15px 0 35px;
    border: 1px solid #ddd;
    border-radius: 18px;
    transition: all 0.3s;
    background-color: #f5f5f5;
    
    &:focus {
      border-color: #1890ff;
      background-color: #fff;
      box-shadow: 0 0 0 2px rgba(24, 144, 255, 0.2);
    }
  }
}

.publish-btn {
  padding: 8px 20px;
  background: linear-gradient(45deg, #1890ff, #36cfc9);
  color: #fff;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
  position: relative;
  z-index: 10;
  box-shadow: 0 2px 8px rgba(24, 144, 255, 0.2);
  min-width: 80px;
  text-align: center;
  
  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(24, 144, 255, 0.3);
  }
}

.c-l-list {
  .c-l-item {
    display: flex;
    padding: 20px;
    border-bottom: 1px solid #eee;
    transition: all 0.3s;
    border-radius: 8px;
    
    &:hover {
      background-color: #fafafa;
      transform: translateX(5px);
    }
    
    .post {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-right: 15px;
      
      .vote {
        cursor: pointer;
        color: #999;
        transition: all 0.3s;
        
        &:hover {
          color: #1890ff;
          transform: scale(1.2);
        }
      }
      
      .text {
        margin: 5px 0;
        font-weight: 500;
        color: #1890ff;
      }
    }
    
    .l-container {
      flex: 1;
      cursor: pointer;
      
      .con-title {
        margin: 0 0 10px;
        font-size: 18px;
        color: #333;
        font-weight: 600;
      }
      
      .con-memo {
        color: #666;
        font-size: 14px;
        line-height: 1.6;
      }
    }
  }
}

.pagination-block {
  margin-top: 20px;
  text-align: center;
}

.run-time-container {
  margin-bottom: 20px;
}

.github-project-card-container {
  margin-bottom: 20px;
}
</style>
