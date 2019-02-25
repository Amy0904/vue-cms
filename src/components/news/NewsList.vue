<template>
  <div>
    
    <ul class="mui-table-view">
      <li class="mui-table-view-cell mui-media" v-for="item in newslist" :key="item.item_id" @click="getnews(item_id)">
        <router-link to="home/newsinfo/">
          <img class="mui-media-object mui-pull-left" :src="item.media_info.avatar_url">
          <div class="mui-media-body">
            <h1>{{ item.abstract }}</h1>
            <p class='mui-ellipsis'>
              <span>发表时间：{{ item.publish_time | dateFormat }}</span>
              <span>点击：{{item.digg_count}}次</span>
            </p>
          </div>
        </router-link>
      </li>

    </ul>

  </div>
</template>
<script>
import { Toast } from "mint-ui";
import jsonp from 'jsonp'
// import func from './vue-temp/vue-editor-bridge';

export default {
  data() {
    return {
      newslist: this.$store.state.newsList // 新闻列表
    }
  },
  created() {
    this.getNewsList()
  },
  methods: {
    getNewsList() {

      return new Promise((resolve, reject) => {
        jsonp('http://ic.snssdk.com/2/article/v25/stream/?count=20&min_behot_time=1504621638&bd_latitude=4.9E-324&bd_longitude=4.9E-324&bd_loc_time=1504622133&loc_mode=5&loc_time=1504564532&latitude=35.00125&longitude=113.56358166666665&city=%E7%84%A6%E4%BD%9C&lac=34197&cid=23201&iid=14534335953&device_id=38818211465&ac=wifi&channel=baidu&aid=13&app_name=news_article&version_code=460&device_platform=android&device_type=SM-E7000&os_api=19&os_version=4.4.2&uuid=357698010742401&openudid=74f06d2f9d8c9664%20---------------------%20%E4%BD%9C%E8%80%85%EF%BC%9Adaimengs%20%E6%9D%A5%E6%BA%90%EF%BC%9ACSDN%20%E5%8E%9F%E6%96%87%EF%BC%9Ahttps://blog.csdn.net/daimengs/article/details/79103138%20%E7%89%88%E6%9D%83%E5%A3%B0%E6%98%8E%EF%BC%9A%E6%9C%AC%E6%96%87%E4%B8%BA%E5%8D%9A%E4%B8%BB%E5%8E%9F%E5%88%9B%E6%96%87%E7%AB%A0%EF%BC%8C%E8%BD%AC%E8%BD%BD%E8%AF%B7%E9%99%84%E4%B8%8A%E5%8D%9A%E6%96%87%E9%93%BE%E6%8E%A5%EF%BC%81', (err,result) => {
          if (err) {
            console.error(err.message);
          } else {       
          resolve(result)
          }
        })
      }).then((result) => {
        console.log(result)
        this.newslist = result.data
        console.log(this.newslist)
      })
    },
    getnews(id){
      this.newslist.forEach((item,index) => {
        if(item.url == id){
          console.log(item)
        }
      });
      if(!localStorage.getItem('newsinfo')){
          // localStorage.setItem('newsinfo',JSON.stringify(this))
      }
    }
  },
  watch:{
    '$store.state.newsList'(){
      this.newslist = this.$store.state.newsList
    }
  }
};




</script>

<style lang="scss" scoped>
.mui-table-view {
  li {
    h1 {
      font-size: 14px;
    }
    .mui-ellipsis {
      font-size: 12px;
      color: #226aff;
      display: flex;
      justify-content: space-between;
    }
  }
}
</style>
