<template>
  <div class="wrapper">
      <div class="back">
        <text class="text" @click="goback"></text>
      </div>
      <image class="image" resize="contain" src="image_src" />
    <div class="second">
      <text class="price">¥{{price}}</text>
      <div class="userinfo" @click="chat">
        <image class="icon" resize="contain" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1527422893435&di=2e679e76e967ba39cb3d5fb9d7545eba&imgtype=0&src=http%3A%2F%2Fimgsrc.baidu.com%2Fforum%2Fw%3D580%2Fsign%3D1588b7c5d739b6004dce0fbfd9503526%2F7bec54e736d12f2eb97e1a464dc2d56285356898.jpg" />
        <text class="username">{{username}}</text>
      </div>
    </div>
    <div class="bookname">
      <text class="text">书名：{{bookname}}</text>
    </div>
    <div class="description">
      <text class="text">描述：{{description}}</text>
    </div>
    <div class="labelzone">
      <text class="text">标签:</text>
      <text class="label" v-for="label in lists":key="label">{{label}}</text>
    </div>
    <div class="buy" @click="trade">
      <text class="buytext">我要购买</text>
    </div>
  </div>
</template>

<script>
  var stream = weex.requireModule('stream');
  var navigator = weex.requireModule('navigator');
  var modal = weex.requireModule('modal');
  module.exports = {
    data:{
      Id:'0',
      bookid:1,
      buyerid:'15331059',
      sellerid:'15331060',
      lists:[
        '课外书','东校区','课外书','东校区'
      ],
      price:'18',
      username:'noname',
      bookname: '小王子',
      image_src:'https://gw.alicdn.com/tfs/TB1dZ4WowoQMeJjy0FnXXb8gFXa-950-1267.jpg',
      description:'好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看好看...'
    },
    methods:{
      goback:function(){
		    this.$router.back(-1);
	  },
      chat:function(){
		    this.$router.push({path:'/chat'});
	  },
      trade:function(){
        stream.fetch({
          method: 'POST',
          url: 'http://123.207.86.98:3000/api/trade/'+this.bookid,
          type:'json',
          headers:{'Content-Type':'application/json'}          
        },
        (ret)=>{
          if(!ret.ok){
            modal.toast({ 
					    'message': "failed",
					    'duration': 2.0
				    });            
          }else{
            modal.toast({ 
					    'message': "交易成功",
					    'duration': 2.0
				    });
          	this.$router.back(-1);
          }
        });
      }
    },
    created(){
      stream.fetch({
        method: 'GET',
        url: 'http://123.207.86.98:3000/api/book/'+this.bookid,
        type:'json',
        headers:{'Content-Type':'application/json'},
      },
      (ret)=> {      
    		if(!ret.ok){
          modal.toast({ 
					  'message': "failed",
					  'duration': 2.0
				  });
        }else{
            this.Id = ret.data.publisherId;
            this.price =  ret.data.price;
            this.username =  ret.data.author;
            this.bookname = ret.data.name;
            this.description =  ret.data.description;
            this.lists = ret.data.comment.split(" ");
            this.image_src=ret.data.img;
          }
        }
      );
    }
  }
</script>

<style scoped>
  .wrapper {
    align-items: center;
  }
  .back{
    display:flex;
    height:50px;
    width:700px;
  }
  .second{
    display:flex;
    flex-direction: row;
    justify-content:space-between;
    width:550px;
    height:66px;
  }
  .userinfo{
    display:flex;
    flex-direction: row;
    justify-content:flex-end;
    width:350px;
    height:66px;   
  }
  .image {
    width: 300px;
    height: 450px;
  }
  .price {
    flex: 0 0 33.3333%;
    font-size: 66px;
    color: rgb(255, 0, 0);
  }
  .icon{
    flex:1;
    width:60;
    height:60;
  }
  .username {
    flex:1;
    font-size: 40px;
    color: #6C6C6C;
  }
  .bookname{
    width:600px;
    height:60px;
  }
  .description{
    width:600px;
    height:250px;
  }
  .labelzone{
    display:flex;
    flex-direction: row;
    align-item:center;
    flex-wrap:wrap;
    width:600px;
  }
  .labels{
    display:flex;
    flex-wrap:wrap;
    width:120px;
    flex-direction: row;

  }
  .label{
    margin-top:10px;
    text-align: center;
    margin-left:20px;
    border:2px solid;
    border-radius:20px;
    color: #00CACA;
    font-size: 35px;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
  }
  .labeltext{
    font-size: 35px;
    color: #00CACA;       
  }
  .text{
    font-size: 40px;
    color: rgb(0, 0, 0);   
  }
  .buy{
    justify-content:center;
    align-items: center;
    width:230px;
    height:100px;
    margin-top:50px;
    border:2px solid;
    border-radius:20px;
    color:#00CACA;
    background: #00CACA; 
  }
  .buytext{
    font-size: 40px;
    color: #E0E0E0;    
  }
</style>