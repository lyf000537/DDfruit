<template>
    <briup-fulllayout title="产品列表">
        <div class="product_list">
            <van-row>
                <!-- 侧边导航 -->
                <van-col :span="4">
                    <van-sidebar v-model="activekey">
                        <van-sidebar-item 
                        v-for="c in categories" 
                        :key="c.id" :title="c.name" 
                        @click="categoryId = c.id">
                        </van-sidebar-item>
                    </van-sidebar>
                </van-col>
                <!-- 右侧产品 -->
                <van-col :span="20">
                    <div class="right-content">
                        <briup-product-item 
                        v-for="p in productCategoryFilter(categoryId)"
                        :key="p.id" 
                        :data="p">
                        </briup-product-item>
                    </div>
                </van-col>
            </van-row>

            <van-row class="car">
                <van-col :span="4">总额<strong>￥{{total}}</strong></van-col>
                <van-col 
                :offset="16" 
                :span="4" 
                @click="toConfirmOrderHandler">立即下单</van-col>
            </van-row>
        </div>
    </briup-fulllayout>
</template>

<script>
import { mapActions, mapState,mapGetters } from 'vuex'
export default {
    data(){
        return {
            activekey:0,
            categoryId:''
        }
    },
    computed:{
        ...mapState('category',['categories']),
        ...mapState("product",["products"]),
        ...mapGetters('product',['productCategoryFilter']),
        ...mapState('shopcar',['orderLines']),
        ...mapGetters('shopcar',['total'])
    },
    created(){
        // 查询所有栏目信息
        this.findAllCategories();
        // 查询所有产品信息
        console.log(this.$route.query.activekey,'0000')
        this.queryProduct({page:0,pageSize:20000000});
        this.categoryId = this.$route.query.id;
        this.activekey = this.$route.query.activekey;
    },
    methods:{
        ...mapActions('category',['findAllCategories']),
        ...mapActions('product',['queryProduct']),
        toConfirmOrderHandler(){
        this.$router.push({path:'/manager/order_confirm'})
        }
    }
}
</script>

<style scoped>
    .car {
        position: fixed;
        bottom: 0;
        line-height: 4em;
        width: 100%;
        background-color: rgba(0, 0, 0, 0.3);
        color: #ffffff;
        text-align: center;
    }
</style>