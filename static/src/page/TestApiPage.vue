<template>
   <div class="g-textApi">
     <p class="title is-5 has-text-centered">
       简单的网络请求
     </p>
     <div class="c-content">
       <div class="field">
         <label class="label">url地址</label>
         <div class="control">
           <input class="input" v-model="url" type="text" placeholder="url">
         </div>
       </div>

       <div class="field">
         <label class="label">请求类型</label>
         <div class="control">
           <div class="select">
             <select v-model="type">
               <option value="get">get</option>
               <option value="post">post</option>
             </select>
           </div>
         </div>
       </div>

       <div class="control">
         <label class="radio">
           <input type="radio" v-model="hasImg" value="1" name="rsvp">
           数据
         </label>
         <label class="radio">
           <input type="radio" v-model="hasImg" value="2"  name="rsvp">
           上传图片
         </label>
       </div>
       <div class="field" v-if="hasImg==1">
         <label class="label">数据</label>
         <div class="control">
           <textarea class="textarea" v-model="data" placeholder="数据"></textarea>
         </div>
       </div>
       <div class="field is-grouped" v-else>
         <div class="control">
           上传图片 <input @change="change"  type="file" />
         </div>

       </div>
       <div class="field is-grouped">
         <div class="control">
           <button class="button is-link" @click="submit">提交</button>
         </div>

       </div>

       <div class="field">
         <label class="label">返回的数据</label>
         <div v-html="resBody">

         </div>
       </div>

     </div>

   </div>
</template>
<style scoped>
  .c-content{
    width: 500px;
    margin: 0 auto;
  }
</style>
<script>

import api from '../api/api'
import axios from 'axios'

   export default {

    data () {
     return {
       dataList:[],
       hasImg:1,
       url:'',
       data:'',
       type:'get',
       formData:new FormData(),
       resBody:''
     }
    },
     watch:{
       hasImg(){
           if(this.hasImg==2){
               this.type = 'post'
           }
       }
     },
     mounted(){


     },
     methods:{
       change(e){
         let file = e.target.files[0];
         this.formData.delete("file");
         this.formData.append("file", file);
         console.dir(this.formData)
       },
        submit(){

           let header = '';
           if(this.hasImg == 2){
              console.dir(this.formData);
             this.formData.delete("url");
             this.formData.append("url", this.url);

             axios({
               method:"POST",
               url:'/api/index/testUploadImg',
               header:{
                 "content-type": "multipart/form-data",
               },
               data:this.formData,
             }).then((res)=>{

               this.resBody = JSON.stringify(res.data, null, 4).replace(/\n/g, '<br>').replace(/\s/g, '&nbsp');

             })
           }else{
               api.index.testApi(this.url,this.type,this.data,header).then((res)=>{
                 console.log(res)
                 this.resBody = JSON.stringify(res.data, null, 4).replace(/\n/g, '<br>').replace(/\s/g, '&nbsp');
               })
             }


        }
     }
  }
</script>

