    <template>
    <div>
    <div v-if="!loadingData">
        <v-header :title="title" :mintit="mintit"></v-header>
        <section class="ui-panel" @click="toList">
               <h2 class="ui-arrowlink">
                   {{datas.title}}
                   <span class="ui-panel-subtitle">{{datas.total}}个</span>
               </h2>
       </section>
       <div class="section">
        <section class="content" v-for="module in modules">
         <router-link v-bind:to="{name: 'show', query: {id: module.id,no:'123'}}">
            <img :src="module.images.large" alt="">
            <p class="ui-nowrap">{{module.title}}</p>
        </router-link>

        </section>
        </div>
    </div>
    <loading :show="loadingData"></loading>
    </div>
    </template>
    <script>
        import axios from 'axios'
        import header from '../components/HeaderBar.vue'
        import loading from '../components/Loading.vue'
        import router from '../router'
        export default{
          data () {
            return {
              title: '电影',
              mintit: {
                name: '列表'
              },
              datas: {},
              modules: {},
              loadingData: true
            }
          },
          methods: {
            toList () {
                // 带查询参数，变成 /register?plan=private
              router.push({name: 'list', query: {type: 'in_theaters'}})
            }
          },
          components: {
            'v-header': header,
            'loading': loading
          },
          mounted: function () {
            // let that = this  ES5用
            axios({
              method: 'post',
              url: '/v2/movie/top250',
              data: {
                count: 16
              }
            }).then(response => {
              console.log(response)
              this.datas = response.data
              this.modules = response.data.subjects
              this.loadingData = false
            }).catch(function (error) { // ES5用  function
              console.log(error)
               // that.datas = response.data
              // this.modules = response.data.subjects
              // this.loadingData = false
            })
          }
        }
    </script>
    <style>
      .ui-panel {
          margin-top: 100px;
          padding-bottom: 5px;
          line-height: 80px;
          font-size: 30px;
          /* display: block; */
      }
      .section{
        display: flex;
        flex-direction: row;
        align-items: flex-end;
        flex-wrap: wrap;
        justify-content: space-around;
      }
      .content a{
          float: left;
      }
      .ui-nowrap{
          line-height: 40px;
          font-size: 30px;
      }
     .ui-grid-trisect li {
          position: relative;
      }
      .ui-grid-trisect p {
          position: absolute;
          bottom: 0px;
          height: 30px;
          line-height: 30px;
          color: #fff;
          width: 100%;
          text-align: center;
          font-size: 14px;
          background: rgba(0,0,0,0.6);
          padding:0px 5px;
      }
    </style>
