<template>
  <div class="seller" v-el:seller>
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <div class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>元
            </div>
          </li>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item border-1px" v-for="item in seller.supports">
            <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
            <span class="text">{{seller.supports[$index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" v-el:pic-wrapper>
          <ul class="pic-list" v-el:pic-list>
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6 ">
  import BScroll from 'better-scroll';
  import star from '../../components/star/star.vue';
  import split from '../../components/split/split.vue';
  export default {
    props:{
        seller:{
            type: Object
        }
    },
    created() {
      //顺序根据data.json里面的support-type里面来的
      this.classMap = ['decrease','discount','special','invoice','guarantee'];
    },
    ready() {
        //当DOM被渲染完后才会执行这个created
//      this.scroll = new BScroll(this.$els.seller,{
//          click:true
//      });
      this._initScroll();
      this._initPics();
    },
    watch: {
      'seller'() {
        this._initScroll();
        this._initPics();
      }
    },
    methods:{
      _initScroll:function(){
        if(!this.scroll){
          this.scroll = new BScroll(this.$els.seller,{
            click:true
          });
        }else{
            this.scroll.refresh();
        }
      },
      _initPics(){
        if(this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth+margin) * this.seller.pics.length - margin;
          this.$els.picList.style.width = width + 'px';
          if(!this.picScroll){
            this.$nextTick(() => {
              this.picScroll = new BScroll(this.$els.picWrapper,{
                scrollX:true,
                // 有些时候你想保留原生纵向的滚动条但想为横向滚动条增加Scroll功能
                eventPassthrough:'vertical'
              })
            });
          }else{
              this.picScroll.refresh();
          }
        }
      }
    },
    components: {
        star:star,
        split:split
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "../../common/sass/common";

  .seller{
    position:absolute;
    top:174px;
    bottom:0;
    left:0;
    width:100%;
    overflow:hidden;
    .overview{
      padding:18px;
      .title{
        font-size:14px;
        line-height: 14px;
        margin-bottom:8px;
        color: rgb(7,17,27);
      }
      .desc{
        padding-bottom:18px;
        font-size:0;
        @include border1px(rgba(7,17,27,0.1));
        .star{
          display: inline-block;
          margin-right: 8px;
          vertical-align: top;
        }
        .text{
          margin-right: 12px;
          display: inline-block;
          line-height: 18px;
          vertical-align: top;
          font-size: 10px;
          color: rgb(77,85,93);
        }
      }
      .remark{
        display: flex;
        margin-top: 18px;
        .block{
          flex:1;
          text-align: center;
          border-right: 1px solid rgba(7,17,27,0.1);
          &:last-child{
            border: none;
          }
          h2{
            margin-bottom: 4px;
            line-height: 10px;
            font-size: 10px;
            color: rgb(147,153,159);
          }
          .content{
            line-height:24px;
            font-size: 10px;
            color: rgb(7,17,27);
            .stress{
              font-size: 24px;
            }
          }
        }
      }
    }
    .bulletin{
      padding:18px 18px 0 18px;
      .title{
        margin-bottom: 8px;
        line-height:14px;
        color: rgb(7,17,27);
        font-size: 14px;
      }
      .content-wrapper{
        padding:0 12px 16px 12px;
        @include border1px(rgba(7,17,27,0.1));
        .content{
          line-height: 24px;
          font-size: 12px;
          color: rgb(240,20,20);
        }
      }
    }
    .supports{
      .support-item{
        padding:16px 12px;
        @include border1px(rgba(7,17,27,0.1));
        font-size: 0;
        &:last-child{
          @include border-none();
        }
      }
      .icon{
        display: inline-block;
        width:16px;
        height:16px;
        vertical-align: top;
        margin-right: 6px;
        background-size: 16px 16px;
        background-repeat: no-repeat;
        &.decrease{
          @include bg-img('decrease_4');
        }
        &.discount{
          @include bg-img('discount_4');
        }
        &.guarantee{
          @include bg-img('guarantee_4');
        }
        &.invoice{
          @include bg-img('invoice_4');
        }
        &.special{
          @include bg-img('special_4');
        }
      }
      .text{
        line-height: 16px;
        font-size: 12px;
        color: rgb(7,17,27);
      }
    }
    .pics{
      padding:18px;
      .title{
        margin-bottom: 12px;
        line-height:14px;
        color: rgb(7,17,27);
        font-size: 14px;
      }
      .pic-wrapper{
        width:100%;
        overflow: hidden;
        white-space: nowrap;
      }
      .pic-list{
        font-size: 0;
        .pic-item{
          display: inline-block;
          margin-right: 6px;
          width:120px;
          height:90px;
          &:last-child{
            margin: 0;
          }
        }
      }
    }
  }
</style>
