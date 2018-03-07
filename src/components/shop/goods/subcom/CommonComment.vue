<template>
    <div class="comment-box">
        <!--取得评论总数-->
        <form id="commentForm" name="commentForm" class="form-box">
            <div class="avatar-box">                                    
                <i class="iconfont icon-user-full"></i>                                    
            </div>
            <div class="conn-box">
                <div class="editor">
                    <textarea id="txtContent" name="txtContent" v-model="txt.txtContent" sucmsg=" " datatype="*10-1000" nullmsg="请填写评论内容！"></textarea>
                    <span class="Validform_checktip"></span>
                </div>
            <div class="subcon">
                <!-- 记得阻止默认提交行为 -->
                <input id="btnSubmit" name="submit" @click="sendComment"  value="提交评论" class="submit">
                <span class="Validform_checktip"></span></div>
            </div>
        </form>
        <ul id="commentList" class="list-box">
            <p  v-if="commentList.length==0" style="margin:5px 0 15px 69px;line-height:42px;text-align:center;border:1px solid #f7f7f7;">暂无评论，快来抢沙发吧！</p>
            <li v-for="item in commentList" :key="item.id">
                <div class="avatar-box">
                    <i class="iconfont icon-user-full"></i>
                </div>
                <div class="inner-box">
                    <div class="info">
                        <span>{{item.user_name}}</span> <span>{{item.add_time | getdate}}</span>
                    </div>
                    <p>{{item.content}}</p>
                </div>
            </li>
        </ul>
    </div>
    
<!--/网友评论-->
</template>

<script>
export default {
    props: ['id'],
    data() {
        return {
            commentList: [],
            txt: {
                txtContent: ''
            }
        }
    },
    methods: {
        getCommentById() {
            this.$http.get(`${this.$api.commentList}goods/${this.id}?pageIndex=1&pageSize=10`).then(res=>{
                if(res.data.status==0){
                    console.log(res);
                    this.commentList=res.data.message;
                }
            })
        },
        
        // 提交评论
        sendComment(){
            this.$http.post(`${this.$api.comment}goods/${this.id}`,this.txt).then(res=>{
                if(res.data.status==0){
                    this.txt.txtContent='';
                    this.getCommentById();
                }
            })
        }
    },
    filters: {
      getdate(tplDate){
          let date= new Date(tplDate);
          return `${date.getFullYear()}/${date.getMonth() +1 }/${date.getDate()} ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`
      }  
    },
    created(){
        this.getCommentById()
    },
    watch: {
        // 监听id是否变化
        id(){
            this.getCommentById()
        }
    }
};
</script>

<style scoped>

</style>