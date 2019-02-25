<template>
  <div>
    <div class="contentright">
      <div class="contentright_box">
        <div class="contentright_box_left fl">
            <div class="contentright_box_left_title">
                <h2 class="fl">公司资讯</h2>
                <a href="#" class="fr">查看更多 +</a>
                <div class="clear"></div>
            </div>
            <ul class="contentright_box_left_ul">
                <li class="contentright_box_left_ul_li" :class="{on:index =='0'}" v-for="(item, index) in newList" :key="index">
                    <a :href="'http://oa.jklife.com'+item.link">
                        <div v-if="index==0">
                          <div class="contentright_box_left_ul_li_img fl">
                            <img :src="item.imgUrl">
                          </div>
                          <div class="contentright_box_left_ul_li_text fl">
                              <h3>{{item.publishDate.split(' ')[0].replace(/-/g ,',')}}</h3>
                              <h2 class="contentright_box_left_ul_li_text_h2">{{item.title}}</h2>
                              <p>{{item.brief}}</p>
                          </div>
                          <div class="clear"></div>
                        </div>
                        <div v-else>
                          <div class="contentright_box_left_ul_li_date fl">
                            <h2>{{item.publishDate.split(' ')[0].split('-')[item.publishDate.split(' ')[0].split('-').length-1]}}</h2>
                            <p>{{item.publishDate.split(' ')[0].slice(0,7).replace('-',',')}}</p>
                          </div>
                          <div class="contentright_box_left_ul_li_conetet">
                              <h2 class="contentright_box_left_ul_li_conetet_h2">{{item.title}}</h2>
                              <p>{{item.brief}}</p>
                          </div>
                          <div class="clear"></div>
                        </div>
                    </a>
                </li>
                <!-- 模板 -->
                <!-- <li class="contentright_box_left_ul_li">
                    <a href="#">
                        <div class="contentright_box_left_ul_li_date fl">
                            <h2>22</h2>
                            <p>2018,05</p>
                        </div>
                        <div class="contentright_box_left_ul_li_conetet">
                            <h2 class="contentright_box_left_ul_li_conetet_h2">君康人寿启用全新品牌标识</h2>
                            <p>1月30日，君康人寿保险股份有限公司下称,1月30日，君康人寿保险股份有限公司下称1月30日，君康人寿保险股份有限公司下称.君康人寿保险股份有限公司下称...</p>
                        </div>
                        <div class="clear"></div>
                    </a>
                </li> -->
            </ul>
        </div>
        <!-- 最新更新  -->
        <div class="contentright_box_right fl">
            <div class="contentright_box_left_title">
                <h2 class="fl">最新更新</h2>
                <a href="#" class="fr">查看更多 +</a>
                <div class="clear"></div>
            </div>
            <ul class="contentright_box_right_ul">
              <li class="contentright_box_right_ul_li" v-for="(item,index) in bulletinList" :key="item.link" :class="{on:item.show}" @click="changeImage(index, item)">
                  <h2 class="contentright_box_right_ul_li_h2">{{item.title}}</h2>
                  <div class="contentright_box_right_ul_li_div" :style="'display: block'" v-show="item.show">
                      <a :href="item.link">
                          <p class="contentright_box_right_ul_li_div_p">{{item.brief}}</p>
                      </a>
                  </div>
              </li>
            </ul>
        </div>
        <div class="clear"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newList: [], // 存放新闻数据
      bulletinList: [], // 存放公告数据
      newTypeID:[], // 存放新闻的ID


    }
  },
  methods: {
    // 最新更新功能实现
    changeImage (index,item) {
      // console.log(index)
      // console.log(item)
      this.bulletinList.forEach(i => {
        if(i.show !== this.bulletinList[index].show){
          i.show = false
          i.changeRed =false
        }
      })
     item.show =!item.show
     item.changeRed=!item.changeRed
    },
    // 公告
     getBulletin() {
        // 最新更新模块渲染
    //    this.axios.get('/seeyon/menhu.do?method=getBulData')
    this.axios.get('/seeyon/menhu.do?method=getNewsList&typeId=1&offset=0&limit=4')

          .then(res => {
            const {items} = res.data
            items.forEach(item => {
              item.show = false
              item.changeRed = false
            })
          //  console.log(res.data)
            this.bulletinList = res.data.items.slice(0,4)
          })
      }
  },
  created(){
    // 获取新闻ID
    //console.log(this.$route);



    this.axios.get('/seeyon/menhu.do?method=getNewsTypeList')

    .then(res => {
    //  console.log(res.data)
      this.newTypeID = res.data.items
    })
    // 公司新闻渲染
    //this.axios.get('/seeyon/menhu.do?method=getNewsList')
   this.axios.get('/seeyon/menhu.do?method=getNewsList&typeId=1&offset=0&limit=10')
      .then(res => {
         console.log(res.data)
        this.newList = res.data.items
      })
    this.getBulletin()
  }
}
</script>

<style>

</style>
