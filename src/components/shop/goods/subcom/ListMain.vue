<template>
   <div class="listMain">
        <div class="section" v-for="item in listGroup" :key="item.catetitle">
            <!--子类-->
            <div class="main-tit">
                <h2>{{item.catetitle}}</h2>
                <p>
                    <router-link to="" v-for="catelist in item.level2catelist" :key="catelist.subcateid">{{catelist.subcatetitle}}</router-link>
                    <a href="/goods/40.html">更多
                    <i>+</i>
                    </a>
                </p>
            </div>
        <!--/子类-->
        <div class="wrapper clearfix">
            <div class="wrap-box">
                <ul class="img-list">
                    <li v-for=" goodslist in item.datas" :key="goodslist.artID">
                        <router-link  :to="{name: 'goodsDetail',params:{id:goodslist.artID}}">
                            <div class="img-box">
                                <img :src="goodslist.img_url">
                            </div>
                            <div class="info">
                                <h3>{{goodslist.artTitle}}</h3>
                                <p class="price"> <b>¥{{goodslist.sell_price}}.00</b>元</p> 
                                <p> <strong>库存 {{goodslist.stock_quantity}}</strong> <span>市场价： <s>{{goodslist.market_price}}.00</s> </span> </p>
                            </div>
                        </router-link>
                    </li>
                </ul>
            </div>
        </div>
    </div>
   </div>
</template>

<script>
    export default {
        data () {
            return {
                listGroup: {
                    catetitle: '',
                    level2catelist: [],
                    datas: []
                }
            }
        },
        methods: {
            getListGroup() {
                this.$http.get(this.$api.goodsContent).then(res=>{
                    if(res.data.status==0){
                        console.log(res);
                        this.listGroup=res.data.message;
                    }
                })
            }
        },
        created () {
            this.getListGroup();
        }
    }
</script>

<style scoped>

</style>