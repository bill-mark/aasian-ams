<!-- 轮播图组件 -->
<template>
<div class="lunbo" v-bind:style="{ height:clientHeights}">
	<div class="super-left" >
			<div class="left">
			    <div class="slide-data">{{slides[nowIndex].title}}</div>
				<div class="slide-show" >
					 <div class="slide-img">
					 	  <a :href="slides[nowIndex].src" target="_blank">
					 	  	  <transition >
					 	  	      <img v-if="isShow" :src="slides[nowIndex].src">
					 	  	  </transition>
					 	  	  <transition >
					 	  	      <img v-if="!isShow" :src="slides[nowIndex].src">
					 	  	  </transition>
					 	  </a>
					 </div>
			    </div>  
			</div>   
			<div class="middle">
			    <div class="middle-form">
			    </div>
				<ul>
				    <li class="li-hpa">hpa</li>
					<li v-for="formli in formLi" 
		                v-bind:class="[checkActive(formli.altitude)?'middle-li-1th-2':'middle-li-1th']" 
		                v-on:click="checkList(formli.altitude)"
					>
						{{formli.altitude}}
					</li>		
				</ul>
			</div>
	</div>
	<div class="right">
	     <div class="right-1th">TIME(UTC)</div>
		 <ul class="right-2th">
		 	<li v-for="(item,index) in slides" 
			 	class="right-li" 
			 	v-bind:style="{'background-color':item.ouhang}"
			 	@click="goto(index)"
			 	:class="{on:index === nowIndex}" 
		 	>
		 		{{item.title}}
		 		<a class="right-a" :href="item.src" target="_blank"></a>
		 	</li>
		 </ul>
		 	<ul class="right-3th">
		 		<li @click="goto(prevIndex)" class="right4th-li right4th-li1th fa fa-backward"></li>
		 		<li v-on:click="runInv" class="right4th-li right4th-li2th fa fa-play"></li>
		 		<li v-on:click="clearInv" class="right4th-li right4th-li3th fa fa-stop"></li>
		 		<li @click="goto(nextIndex)" class="right4th-li right4th-li4th fa fa-forward"></li>
		 	</ul>
	</div>
</div>	
</template>

<script>
import _ from 'lodash'
export default {
	props: {
		slides: {
			type: Array,
			default: []
		},
		inv: {
			type: Number,
			default: 1000
		},
		formLi:{
			type:Array,
			default:[]
		}
	},
	data () {
		return {
			nowIndexes:[],
			nowIndex: 0,
			isShow: true,
			clientHeights:null
		}
	},
	methods: {
		goto (index) {
			 this.isShow = false
			 setTimeout(()=>{
                 this.isShow = true
                 this.nowIndex = index
			 },10)			 
		},
		runInv () {
			this.invId = setInterval(()=>{
				this.goto(this.nextIndex)
			},this.inv)
		},
		clearInv () {
           clearInterval(this.invId)  
		},
		checkList: function(index) {
			//alert(this.formLi[0].altitude)

			if(this.nowIndexes.indexOf(index) === -1 ){
                //调用父组件change-hpa方法，进行更新slides
                this.$emit('change-hpa',index)

				this.nowIndexes=[]  
				this.nowIndexes.push(index)
			}
			else{		
				this.nowIndexes = _.remove(this.nowIndexes,(idx)=>{
	               return idx !== index
	          })
			}
		},
		checkActive (index) {
		   return	this.nowIndexes.indexOf(index) != -1 
		},
		gaodu (){
			this.clientHeights = `${window.innerHeight-92}px`;
			const that = this
			window.onresize = function temp() {
			    that.clientHeights = `${window.innerHeight-92}px`;
			}
		},
		changelist (){
			this.slides.forEach(function(item,index){
				if(index%2==0){
			         item.ouhang = 'rgb(247,248,250)'
				}
			})
		}
	},
	computed : {  //计算属性，绑定轮播图左右翻页
		prevIndex () {
			if(this.nowIndex === 0) {
				 return this.slides.length -1
			}
			else {
				 return this.nowIndex -1
			}
		},
		nextIndex () {
			 if(this.nowIndex === this.slides.length-1){
			 	  return 0
			 }
			 else {
			 	   return this.nowIndex +1
			 }
		}      
	},
	mounted () { //组件渲染完毕执行该函数
		//console.log(this.slides)
		//this.runInv()
        //this.nowIndex.push(hpa);
        	
        this.gaodu()
        this.changelist()
	}
}
</script>

<style scoped>
.lunbo{
  display: flex;
  flex-flow: row;
  
}
.li-hpa{
  color: #ccc;
  padding-left: 20px;
}
.super-left{
  min-width: 866px;	
  flex: 1;
  border: 8px solid rgb(238,238,238);
  border-radius: 16px;
  margin-top: 9px;
  position: relative;
  background-color: white;
  min-height: 725px;
}
.left{
	min-width: 780px;
    width: calc(100% - 65px);
}
.middle{
    width: 55px;
    min-height: 758px;
    position: absolute;
    right: 10px;
    height: 100%;
    top:3%;
}
.middle-form {
	width: 8px;
	border: 2px solid rgb(212,216,223);
	height: 732px;
	float: left;
	background: rgb(226,230,237);
	height: 90%;
}
.middle-li-1th{
  margin-top: 26px;
  -webkit-margin-top: 19px;
  -moz-margin-top:22px;
  position: relative;
  cursor: pointer;
  font-size: 16px;
  color: #ccc;
  padding-left: 20px;
  width: 42px;
}
.middle-li-1th:after{
	content:"";
	display:block;
	width:6px;
	height:6px;
	background:white;
	position:absolute;
	border-radius: 3px;
	left:4px;
	top: 5px;
}
.middle-li-1th-2{
	margin-top: 26px;                   
	position:relative;
	cursor: pointer;
	padding-left: 20px;
	width: 42px;
}
.middle-li-1th-2:after{
	content: "";
	display: block;
	width: 25px;
	height: 26px;
	background: white;
	position: absolute;
	left: -3px;
	top: -1px;
	background: url("../assets/slideShow/yuan.png") no-repeat;
	background-size: 24px 24px;
	background-position: -2px 0px;
}
.slide-data{
    color: #3366cc;
    bottom: 0;
    height: 25px;
    padding-top: 7px;
    text-align: center;
    font-size: 14px;
    font-weight: bold;
}
.slide-data2{
	width: 25px;
}
.slide-trans-enter-active {
  transition: all .5s;
}
.slide-trans-enter {
  transform: translateX(900px);
}
.slide-trans-old-leave-active {
  transition: all .5s;
  transform: translateX(-900px);
}
.slide-show {
  width: 770px;
  height: 620px;
}
.slide-show h2 {
  position: absolute;
  width: 100%;
  height: 100%;
  bottom: 0;
  height: 30px;
  text-align: left;
  padding-left: 15px;
}
.slide-img {
  width: 100%;
  height: 100%;
}
.slide-img img {
  width: calc(100% - 65px);
  height: calc(100% - 40px);
  position: absolute;
}
.slide-pages {
  position: absolute;
  bottom: 10px;
  right: 15px;
}
.slide-pages li {
  display: inline-block;
  padding: 0 10px;
  cursor: pointer;
  color: #363636;
  font-size: 14px;
}
.on {
 background: #4fc08d !important;
}
.ouhang {
	background-color: rgb(247,248,250);
}	
.right{
    width: 260px;
    min-width: 260px;
    border: 8px solid rgb(238,238,238);
    border-radius: 16px;
    margin-top: 9px;
    margin-left: 20px;
    background-color: white;
    /*margin-right: 10px;*/
}
.right-1th{
	height: 25px;
	line-height: 25px;
	text-align: center;
	border-bottom: 5px solid rgb(238,238,238);
	color: #336666;
	font-size: 14px;
}
.right-2th{
	height: calc(100% - 100px);
    border-bottom: 3px solid rgb(238,238,238);
}
.right-li{
    position: relative;
    padding-top: 12px;
    padding-bottom: 12px;
    padding-left: 58px;
    cursor: pointer;
    font-size: 12px;
    letter-spacing: 1px;
}
.right-a{
   background: url("../assets/fangda.png") no-repeat;
   position: absolute;
   width: 17px;
   height: 17px;
   left: 225px;
}
.right-3th{
  display: inline-block;
}
.right3th-left{
    float: left;
	width: 62px;
	border: 2px solid rgb(62,77,110);
	height: 23px;
	margin-top: 10px;
	margin-left: 29px;
	line-height: 23px;
	text-align: center;
	cursor: pointer;
}
.right3th-right{
    width: 62px;
    border: 2px solid rgb(94,160,44);
    height: 23px;
    color: rgb(94,160,44);
    margin-left: 120px;
    margin-top: 10px;
    text-align: center;
    line-height: 23px;
    cursor: pointer;
}
.right4th-li{
	float: left;
	border: 1px solid rgb(216,216,216);
	width: 29px;
	height: 25px;
	margin-left: 27px;
	margin-top: 13px;
	cursor: pointer;
	text-align: center;
	line-height: 25px;
}
.right4th-li1th{
      
}
.right4th-li2th{
   
}
.right4th-li3th{
    
}
.right4th-li4th{
    
}
.right-4th{
	height: 120px;
}
</style>