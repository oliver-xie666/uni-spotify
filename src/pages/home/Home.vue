<template>
  <u-template>
    <u-container class="home-wrapper">
      <!-- 大卡片 -->
      <u-card hover class="large-card" :data="largeCard">
        <view class="title" @click.stop="jumpToPlaylist(largeCard.id)">{{
          largeCard.title
        }}</view>
        <u-flex justify="between">
          <u-image
            :src=" largeCard.creator.avatar"
            :title="largeCard.creator.name"
          ></u-image>
          <u-button circle mini>三</u-button>
        </u-flex>
        <u-list>
          <u-list-item
            each="{{ largeCard.songs }}"
            :key="song.id"
            :data="song"
            @click.stop="playMusic(song.id)"
            >{{ song.title }}</u-list-item
          >
        </u-list>
      </u-card>
      <!-- 小卡片 -->
      <u-card
        hover
        style="margin-top: 10px"
        :each="recommendLists"
        :key="list.id"
        :data="list"
        @click.stop="jumpToPlaylist(list.id)"
        :title="list.title"
      >
        <u-flex justify="around">
          <u-image
            mode="aspectFill"
            style="height: 48px; width: 48px"
            :src="list.coverImgUrl"
          ></u-image>
          <!-- 根据列表中歌曲不同返回不同内容 -->
          <view v-if="list.songs && list.songs.length > 0">
            <u-text class="recommend-list-text" numberOfLines="1" ellipsis>{{
              list.songs[0].title
            }}</u-text>
            <u-text class="recommend-list-text" numberOfLines="1" ellipsis>{{
              list.songs[1].title
            }}</u-text>
          </view>
          <view v-else-if="list.trackCount">
            <u-text class="recommend-list-text"
              >{{ list.trackCount }}首单曲歌曲
              {{ list.playCount | number }}次收听</u-text
            >
          </view>
        </u-flex>
      </u-card>
    </u-container>
  </u-template>
</template>
<script>
import { fetchData } from "../../utils/api";

export default {
  data() {
    return {
      // Spotify 首页的大小卡片信息
      largeCard: {
        id: 0,
        type: "",
        title: "",
        songs: [],
        creator: {
          name: "",
          avatar: "",
        },
      },
      // spotify 上的推荐列表
      recommendLists: [],
    };
  },
  mounted() {
    this.getLargeCards();
    this.getRecommendLists();
  },
  methods: {
    // 获取推荐卡片信息
    async getLargeCards() {
      const res = await fetchData("/card");
      if (res.msg === "ok") {
        this.largeCard = {
          ...res.data,
        };
      }
    },
    // 获取推荐列表
    async getRecommendLists() {
      const res = await fetchData("/recommendLists");
      if (res.msg === "ok") {
        this.recommendLists = [...res.data];
      }
    },
    // 播放 音乐
    playMusic(songId) {
      this.$router.push({
        name: "player",
      });
    },
  },
};
</script>
<style scoped>
</style>