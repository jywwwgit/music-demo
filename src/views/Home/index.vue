<template>
  <div>
    <p class="title">推荐歌单</p>
    <div>
      <van-row gutter="6">
        <van-col span="8" v-for="obj in reList" :key="obj.id">
          <van-image width="100%" height="3rem" fit="cover" :src="obj.picUrl" />
          <p class="song_name">{{ obj.name }}</p>
        </van-col>
      </van-row>
    </div>
    <p class="title">最新音乐</p>
   <!-- <van-cell center v-for="obj in songList" :key="obj.id" :title="obj.name" :label="obj.song.artists[0].name + ' - ' + obj.name">
      <template #right-icon>
        <van-icon name="play-circle-o" size="0.6rem" />
      </template>
    </van-cell> -->
    <SongItem v-for="obj in songList"
    :key="obj.id"
    :name="obj.name"
    :author="obj.song.artists[0].name"
    :id="obj.id"></SongItem>
  </div>
</template>

<script>
import { recommendMusicAPI,newMusicAPI } from "@/api";
import SongItem from "@/components/SongItem.vue"
export default {
  components: {
    SongItem
  },
  data() {
    return {
      reList: [],
      songList: []
    };
  },
  async created() {
    const res = await recommendMusicAPI({
      limit: 6,
    });
    // console.log(res);
    this.reList = res.data.result;
    const res2 = await newMusicAPI({
      limit: 20
    })
    console.log(res2);
    this.songList = res2.data.result
  },
};
</script>

<style>
/* 标题 */
.title {
  padding: 0.266667rem 0.24rem;
  margin: 0 0 0.24rem 0;
  background-color: #eee;
  color: #333;
  font-size: 15px;
}

/* 推荐歌单 - 歌名 */
.song_name {
  font-size: 0.346667rem;
  padding: 0 0.08rem;
  margin-bottom: 0.266667rem;
  word-break: break-all;
  text-overflow: ellipsis;
  display: -webkit-box;
  /** 对象作为伸缩盒子模型显示 **/
  -webkit-box-orient: vertical;
  /** 设置或检索伸缩盒对象的子元素的排列方式 **/
  -webkit-line-clamp: 2;
  /** 显示的行数 **/
  overflow: hidden;
  /** 隐藏超出的内容 **/
}
/* 给单元格设置底部边框 */
.van-cell {
  border-bottom: 1px solid lightgray;
}
</style>