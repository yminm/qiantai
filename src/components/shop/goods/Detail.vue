<template>
    <div>
        <!-- 导航栏 -->
        <div class="section">
            <div class="location">
                <span>当前位置：</span>
                <router-link :to="{name: 'goodsList'}">首页</router-link> &gt;
                <router-link :to="{name: 'goodsList'}">商品详情</router-link>
            </div>
        </div>
 
        <!-- 商品详情 -->
        <div class="section">
            <div class="wrapper clearfix">
                <div class="wrap-box">
                    <!--页面左边-->
                    <div class="left-925">
                        <div class="goods-box clearfix">
                            <!--商品图片-->
                            <div class="pic-box">
                                <div class="magnifier" id="magnifier1">
                                    <div class="magnifier-container">
                                        <div class="images-cover"></div>
                                        <!--当前图片显示容器-->
                                        <div class="move-view"></div>
                                        <!--跟随鼠标移动的盒子-->
                                    </div>
                                    <div class="magnifier-assembly">
                                        <div class="magnifier-btn">
                                            <span class="magnifier-btn-left">&lt;</span>
                                            <span class="magnifier-btn-right">&gt;</span>
                                        </div>
                                        <!--按钮组-->
                                        <div class="magnifier-line">
                                            <ul class="clearfix animation03">
                                                <li v-for="item in goodsdetail.imglist" :key="item.id">
                                                    <div class="small-img">
                                                        <img :src="item.original_path" />
                                                    </div>
                                                </li>
                                            </ul>
                                        </div>
                                        <!--缩略图-->
                                    </div>
                                    <div class="magnifier-view"></div>
                                    <!--经过放大的图片显示容器-->
                                </div>
                            </div>
                            <!--/商品图片-->
            
                            <!--商品信息-->
                            <div class="goods-spec">
                                <h1>{{goodsdetail.goodsinfo.title}}</h1>
                                <p class="subtitle">{{goodsdetail.goodsinfo.sub_title}}</p>
                                <div class="spec-box">
                                    <dl> <dt>货号</dt> <dd id="commodityGoodsNo">{{goodsdetail.goodsinfo.goods_no}}</dd> </dl> 
                                    <dl> <dt>市场价</dt> <dd> <s id="commodityMarketPrice">¥{{goodsdetail.goodsinfo.market_price}}.00</s> </dd> </dl>
                                    <dl> <dt>销售价</dt> <dd> <em class="price" id="commoditySellPrice">¥{{goodsdetail.goodsinfo.sell_price}}.00</em> </dd> </dl>
                                </div>
                
                                <div class="spec-box">
                                    <dl>
                                        <dt>购买数量</dt>
                                        <dd>
                                            <div class="stock-box">
                                                <el-input-number size="small" v-model="num"></el-input-number>
                                            </div>
                                            <span class="stock-txt">
                                                库存
                                                <em id="commodityStockNum">{{goodsdetail.goodsinfo.stock_quantity}}</em>件
                                            </span>
                                        </dd>
                                    </dl>
                                    <dl>
                                        <dd>
                                            <div class="btn-buy" id="buyButton">
                                                <button class="buy">立即购买</button>
                                                <button class="add">加入购物车</button>
                                            </div>
                                        </dd>
                                    </dl>
                                </div>
        
                            </div>
                            <!--/商品信息-->
                        </div>
        
                        <div id="goodsTabs" class="goods-tab bg-wrap">
                            <!--选项卡-->
                            <el-tabs type="border-card">
                                <el-tab-pane label="商品介绍">
                                    <div v-html="goodsdetail.goodsinfo.content"></div>
                                </el-tab-pane>
                                <el-tab-pane label="商品评论">
                                    <!-- 将商品id传过去 通过id获取商品评论 -->
                                    <app-comment :id="id"></app-comment>
                                </el-tab-pane>
                            </el-tabs>
                            <!--/选项卡-->
                            
                          
                            <!--/网友评论-->
                            </div>
                            
                        </div>
                    </div>
                    <!-- 右边 推荐商品 -->
                    <app-aside :list="goodsdetail.hotgoodslist"></app-aside>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import '@/lib/imgzoom/css/magnifier.css'
import '@/lib/imgzoom/js/magnifier.js'
import $ from 'jquery'
import AppComment from './subcom/CommonComment'
import AppAside from './subcom/CommonAside'
export default {
    components: {
        AppComment,
        AppAside
    },
    data() {
        return {
            id: this.$route.params.id,
            num: 1,
            goodsdetail: {
                goodsinfo: "",
                imglist: "",
                hotgoodslist: ""
            }
        };
    },
    methods: {
        getDetailById() {
            this.$http.get(this.$api.goodsDetail + this.id).then(res => {
                if (res.data.status == 0) {
                console.log(res.data);
                this.goodsdetail = res.data.message;
                }
            });
        }
    },
    // 页面加载完执行 
    created() {
        this.getDetailById();
    },



//   商品详情页面的右侧列表，可以点击的切换不同的商品进行预览
// 但是默认情况下当前页面切换到当前页面不会触发组件的重新渲染 为了解决这个问题 我们可以监听$route对象的变化 
// 因为切换商品后 $route.params.id变化了 如果变化 就主动发起http请求 调用接口获取新id的数据进行视图刷新
// 
watch: {
    $route(){
        this.id = this.$route.params.id;
        this.getDetailById();
    },
    goodsdetail(){
         var magnifierConfig = {
            magnifier : "#magnifier1",//最外层的大容器
            width : 380,//承载容器宽
            height : 380,//承载容器高
            moveWidth : null,//如果设置了移动盒子的宽度，则不计算缩放比例
            zoom : 5//缩放比例
        };

        setTimeout(() => {
            var _magnifier = $().imgzoon(magnifierConfig);
        }, 500);
    }
}
};
</script>

<style scoped>

</style>