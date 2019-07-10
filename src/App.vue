<template>
  <div>
    <header></header>
    <main>
      <div class="search-wrapper">
        <div class="search">
          <input v-model="songName" />
          <svg class="icon" @click="getSongList">
            <use xlink:href="#icon-search" />
          </svg>
        </div>
      </div>
      <div class="songList">
        <ul v-if="show">
          <div class="title"><span>歌曲</span><span>歌手</span></div>
          <li v-for="(result,index) in results" :key="index">
            <span>{{result.title}}</span>
            <span>{{result.author}}</span>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>
<script>
import "./svg/svg.js";
export default {
  data() {
    return {
      results: [],
      songName: "真心英雄",
      show: false
    };
  },
  mounted() {
    this.getSongList()
  },
  methods: {
    getSongList() {
      this.$http
        .get(`https://api.apiopen.top/searchMusic?name=${this.songName}`)
        .then(response => {
          this.results = response.data.result;
          this.show = true;
        });
    }
  }
};
</script>
<style  lang="scss" scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
ul,ol{
  list-style: none;
}
header {
  background: #fff;
  height: 90px;
}
main {
  .search-wrapper {
    background: #107f63;
    height: 250px;
    display: flex;
    justify-content: center;
    align-items: center;
    .search {
      border: 1px solid #ccc;
      display: flex;
      align-items: center;
      border-radius: 4px;
      input {
        border: none;
        height: 48px;
        width: 544px;
        padding: 10px;
        font-size: 16px;
        outline: none;
      }
      .icon{
        width: 40px;
        height: 48px;
        padding: 2px;
        fill: #111;
        background: #fff;
        &:hover{
          cursor: pointer;
        }
      }
    }
    &:focus{
      border-color: #111;
    }
  }
  .songList{
    width: 956px;
    position: relative;
    left: 50%;
    transform: translateX(-50%);
    .title{
      background: #F7F7F7;
      span{
        display: inline-block;
        height: 36px;
        line-height: 36px;
        &:first-child{
          padding-left: 16px;
          width: 400px;
        }
      }
    }
  }
  ul{
    li{
      padding: 4px 0;
      span{
        display: inline-block;
        height: 36px;
        line-height: 36px;
        font-size: 14px;
        &:first-child{
          padding-left: 16px;
          width: 400px;
          color: #31c27c;
        }
      }
    }
  }
}
</style>
