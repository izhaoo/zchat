<template>
  <div class="container friend">
    <header>
      <nav-bar title="好友" />
    </header>
    <section class="list-section">
      <van-search placeholder="搜索" v-model="keyword" @search="onSearch" />
      <van-skeleton title avatar :row="3" :loading="loading">
        <van-index-bar>
          <van-index-anchor>朋友</van-index-anchor>
          <van-cell
            v-for="item in list"
            :key="item.id"
            :title="item.nickname"
            :to="{ name: 'chat', params: {uid: item._id}}"
            :icon="item.avatar[0].content"
            clickable
            size="large"
          />
        </van-index-bar>
      </van-skeleton>
    </section>
    <section class="list-section">
      <van-skeleton title avatar :row="3" :loading="loading">
        <van-index-bar>
          <van-index-anchor>AI机器人</van-index-anchor>
          <van-cell
            title="小兆同学"
            to="robot"
            :icon="require('@/static/avatar/robot.jpg')"
            clickable
            size="large"
          />
        </van-index-bar>
      </van-skeleton>
    </section>
    <section class="list-section">
      <van-skeleton title avatar :row="3" :loading="loading">
        <van-index-bar>
          <van-index-anchor>小助手</van-index-anchor>
          <van-cell
            title="快递查询"
            to="express"
            :icon="require('@/static/avatar/express.jpg')"
            clickable
            size="large"
          />
          <van-cell
            title="天气查询"
            to="weather"
            :icon="require('@/static/avatar/weather.jpg')"
            clickable
            size="large"
          />
        </van-index-bar>
      </van-skeleton>
    </section>
    <footer>
      <tabbar />
    </footer>
  </div>
</template>

<script>
import { Search, IndexBar, IndexAnchor, Cell, Skeleton } from "vant";
import { Tabbar, NavBar } from "@/components";
import { fetchList } from "@/api/user";
import { mapGetters } from "vuex";

export default {
  name: "Frined",
  components: {
    Tabbar,
    NavBar,
    [Search.name]: Search,
    [IndexBar.name]: IndexBar,
    [IndexAnchor.name]: IndexAnchor,
    [Cell.name]: Cell,
    [Skeleton.name]: Skeleton
  },
  computed: {
    ...mapGetters(["user"])
  },
  data() {
    return {
      list: {},
      keyword: "",
      loading: true
    };
  },
  created() {
    this.getList();
  },
  methods: {
    getList() {
      fetchList().then(res => {
        this.list = res.data.items;
        // 去除自己
        this.list.splice(
          this.list.findIndex(item => item._id == this.user.id),
          1
        );
      });
      this.loading = false;
    },
    onSearch() {
      this.$router.push({
        name: "search",
        params: { type: "friend", keyword: this.keyword }
      });
    }
  }
};
</script>

<style lang="scss" scope>
.van-cell {
  line-height: 32px;
}
.van-icon__image {
  font-size: 32px;
  img {
    border-radius: 50%;
  }
}
.friend {
  overflow: scroll;
  height: calc(100vh - 96px);
  .list-section {
    .van-index-anchor {
      color: #1989fa;
    }
  }
}
</style>
