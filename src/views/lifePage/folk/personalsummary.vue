<template>
    <div class="personsum">
        <Hearder style="position: absolute" />
        <div class="imgbox pt110 clearfix">
            <div class="fll" v-for="(item,index) in imgs" :key=index >
                <img v-lazy="item" alt="">
            </div>
            <label class="upload">
                <input @change="uploadfile" type="file">
            </label>
        </div>
        <div class="button">
            <button @click="handleSublit">提交审核</button>
        </div>
    </div>
</template>

<script>
import Hearder from '@/components/Hearder'
import axios from 'axios'
import { Toast } from 'mint-ui';
    export default {
        components:{
            Hearder
        },
        data(){
            return{
                imgs:[],
                token:''
            }
        },
        methods:{
            getToken(){
                axios.get(`http://mawenli.xyz:3000/getToken`).then(res => {
                    this.token = res.data.data
                })
            },
            uploadfile(e){
                let file = e.target.files[0]
                let fordata = new FormData();
                fordata.append('file',file)
                fordata.append('token',this.token)
                axios.post('https://upload-z1.qiniup.com',fordata).then(res => {
                    this.imgs.push(res.data.url)
                })
            },
            handleSublit(){
                if(this.imgs.length == 0){
                    Toast('请先上传文件')
                }else{
                    this.$axios.post('/ddyj/summary',{contents:this.imgs}).then(res => {
                        Toast(res.msg)
                        // if(res.code == 200){
                        //     this.$router.replace('/folkhome')
                        // }
                    })
                }
            }
        },
        created(){
            this.getToken()
        }
        
    }
</script>

<style scoped lang='less'>
html{
    height: 100%;
}
body{
    height: 100%;
}
#app{
    height: 100%;
}
.personsum{
    display: flex;
    flex-direction: column;
    height: 100%;
    overflow: hidden;
}
.button{
    padding: 0.3rem 0.2rem ;
    button{
        width: 100%;
        height: 0.8rem;
        left: 0.8rem;
        background: #c50206;
        color: #fff;
        border: none;
        border-radius: 4px;
    }
}
.imgbox{
    // height: 10rem;
    flex: 1;
    background: #f1f1f1;
    overflow: auto;
    img{
        width: 2rem;
        height: 2rem;
        margin: 0.16rem;
    }
}
.upload{
    float: left;
    width: 2rem;
    height: 2rem;
    margin: 0.16rem;
    background-color: #ff0;
    background: url('/static/img/upload.png') no-repeat;
    background-size: 100%;
    input{
        display: none;
    }
}
</style>