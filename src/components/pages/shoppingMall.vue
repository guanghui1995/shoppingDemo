<template>
    <div>
        <!-- search area -->
        <div class="search-bar">
            <van-row>
                <van-col span="3"><img :src="location" width="100%" alt=""></van-col>
                <van-col span="16"><input type="text" class="search-input"></van-col>
                <van-col span="5"><van-button size="mini" class="search-button">查找</van-button></van-col>
            </van-row>
        </div>


        <!-- swipe  area-->
        <div class="swipe-banner">
            <van-swipe :autoplay="1000">
                <van-swipe-item v-for="(item,index) in bannerPicArr" :key="index">
                    <img v-lazy="item.image" width="100%">
                </van-swipe-item>    
            </van-swipe>   
        </div>


        <!-- type bar -->
        <div class="type-bar">
            <div v-for="(cate,index) in categroy" :key='index'>
                <img v-lazy="cate.image" alt="" width="90%">
                <span>{{cate.mallCategoryName}}</span>
            </div>
        </div>

        <!-- adBanner area -->
        <div>
            <img v-lazy="adBanner" alt="" width="100%">
        </div>

        <!-- recommend goods area -->
        <div class="recommend-area">
            <div class="recommend-title">商品推荐</div>
            <div class="recommend-body">
                <swiper :options="swiperOption">
                    <swiper-slide v-for="(item,index) in recommendGoods" :key="index">
                        <div class="recommend-item">
                            <img :src="item.image" alt="" width="80%">
                            <div>{{item.goodsName}}</div>
                            <div>￥{{item.price | moneyFiltes}}(￥{{item.mallPrice | moneyFiltes}})</div>
                        </div>
                    </swiper-slide>
                </swiper>
            </div>
        </div>

        <!-- floor area -->
        <floorComponent :floorData='floor1' :floorTitle='floorTitle.floor1'></floorComponent>
        <floorComponent :floorData='floor2' :floorTitle='floorTitle.floor2'></floorComponent>
        <floorComponent :floorData='floor3' :floorTitle='floorTitle.floor3'></floorComponent>


        <!-- hot area -->
        <div class="hotp-area">
            <div class="hot-title">热卖商品</div>
            <div class="hot-goods">
                <van-row gutter="20">
                    <van-col span="12" v-for="(item , index) in hotGoodsList" :key="index">
                        <goodsInfo :goodsImage="item.image" :goodsName="item.name" :goodsPrice="item.price"></goodsInfo>
                    </van-col>
                </van-row>
            </div>
        </div>
    </div> 
</template>

<script>
    import axios from 'axios'
    import 'swiper/dist/css/swiper.css'
    import {swiper, swiperSlide} from 'vue-awesome-swiper'
    import floorComponent from '../component/floorComponent'
    import goodsInfo from '../component/goodsInfoComponent'
    import {newMoney} from '@/filter/moneyFilter.js'
    import url from '@/serviceAPI.config.js'
    export default {
        data()  {
            return {
                msg: 'shopping Mall',
                location: require('../../assets/images/location.png'),
                bannerPicArr:[],
                categroy:[],
                adBanner:'',
                recommendGoods:[],
                floor1:[],
                floor2:[],
                floor3:[],
                floorTitle:{},
                hotGoodsList:[],
                swiperOption:{
                    loop:true,
                    slidesPerView:3
                }

            }
        },
        components:{swiper,swiperSlide,floorComponent,newMoney,goodsInfo},
        filters:{
            moneyFiltes:function(money){
                return newMoney(money)
            }
        },
        created(){
            axios({
                url:url.getshoppingMallInfo,
                method:'get',
            })
            .then(res=>{
                if(res.status == 200){
                   res.data.data.category[0].image=require('../../assets/images/xxsg.png')
                   this.categroy = res.data.data.category;
                   this.adBanner = res.data.data.advertesPicture.PICTURE_ADDRESS;
                   this.bannerPicArr = res.data.data.slides;
                   this.recommendGoods = res.data.data.recommend;
                   this.floor1 = res.data.data.floor1;
                   this.floor2 = res.data.data.floor2;
                   this.floor3 = res.data.data.floor3;
                   this.floorTitle = res.data.data.floorName;    
                   this.hotGoodsList = res.data.data.hotGoods;              
                console.log(res);
                }
            })

            .catch(error=>{
                console.log(error);
            })
        }
    }
</script>

<style scoped>
    .search-bar{
        height: 2.2rem;
        background-color: #e5017d;
        line-height: 2.2rem; 
        overflow: hidden;
    }
    .search-input{
        width: 100%;
        height: 1.3rem;
        border-top:0px;
        border-left: 0px;
        border-right: 0px;
        border-bottom: 1px solid #fff !important;
        background-color: #e5017d;
        color: #fff;
    }
    .search-button{
        border-radius: 5px;
    }
    .swipe-banner{
        clear: both;
        max-height:15rem;
        overflow: hidden;
    }
    .type-bar{
        background-color: #fff;
        margin: 0 .3rem .3rem .3rem;
        font-size: 14px;
        border-radius: .3rem;
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
       
    }
    .type-bar div{
        padding: .3rem;
        font-size: 12px;
        text-align: center;
    }
    .recommend-area{
        background-color: #fff;
        margin-top: .3rem;
    }
    .recommend-title{
        border-bottom: 1px solid #eee;
        font-size: 14px;
        padding: .2rem;
        color: #e5017d;
    }
    .recommend-body{
        border-bottom: 1px solid #eee;
    }
    .recommend-item{
        width: 99%;
        border-right: 1px solid #eee;
        font-size: 10px;
        text-align: center;
    }
    .hotp-area{
        text-align: center;
        font-size: 14px;
        height: 1.8rem;
        line-height: 1.8rem;
    }
</style>