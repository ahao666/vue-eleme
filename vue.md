


## sass 使用
		
		1. 安装sass的依赖包

			npm install --save-dev sass-loader
			//sass-loader依赖于node-sass
			npm install --save-dev node-sass

		2. 在build文件夹下的webpack.base.conf.js的rules里面添加配置
			{
			  test: /\.sass$/,
			  loaders: ['style', 'css', 'sass']
			}

		3. 在APP.vue中修改style标签
			<style lang="scss">


## axios 使用 

		axios 改写为 Vue 的原型属性 
		
		import axios from 'axios'
		Vue.prototype.$ajax= axios	
	
		this.$ajax.get('api/getNewsList').then((response)=>{
	        this.newsList=response.data.data;
	      }).catch((response)=>{
	        console.log(response);
	      })






## vue：Uncaught TypeError：无法读取未定义的属性   

	[https://stackoverflow.com/questions/41051972/vue-uncaught-typeerror-cannot-read-property-of-undefined](https://stackoverflow.com/questions/41051972/vue-uncaught-typeerror-cannot-read-property-of-undefined "具体答案")
		
		在本地开发时，我经常看到警告Uncaught TypeError: Cannot read property ... of undefined，但HTML可以成功呈现。
		但是，将HTML部署到带有npm run build命令的Netlify时，将无法呈现HTML 。所以我必须认真对待这个警告。
		
		我从这里了解到，这是因为“组件渲染时数据不完整，而是例如从API加载数据”。解决方案是“ v-if仅在数据加载完成后才使用该模板部分”。

		<div class="goods" v-if="allData.goods">
        	<div class="menu-wrapper">{{getallData.goods[0].name}}</div>
        	<div class="foods-wrapper"></div>
    	</div>

		v-if每个拥有AJAX调用的组件放在首位。





## created mounted updated 



## $nextTick()


## Vue.set();



## 实例属性：
	
	$props:  当前组件接收到的 props 对象。Vue 实例代理了对其 props 对象属性的访问。

![](https://i.imgur.com/3WQYfJV.png)



## transition-group 
	
	<transition-group> 元素作为多个元素/组件的过渡效果。<transition-group> 渲染一个真实的 DOM 元素。默认渲染 
	<span>，可以通过 tag 属性配置哪个元素应该被渲染。

	注意，每个 <transition-group> 的子节点必须有 独立的 key ，动画才能正常工作




## 父子组件接收派发事件

	父组件接收子组件事件
	 

	child1.vue   $emit(事件名,方法)
	
			addCart(event) {
                if(event.__constructed) {
                    return;
                }
                if(!this.food.count) {
                    // 全局设置 this.food对象 添加 count 属性 默认值为1;
                    Vue.set(this.food,'count',1);
                }else {
                    this.food.count ++;
                }

                this.$emit('add',event.target);  !!! 主要是 这一行
            },


	parent.vue 

		<template>
			// @add="_drop" add是子组件定义的  _drop则是父组件定义的方法用来接收
			<child1 @add="_drop"></child>  

			<child2 ref="child2"></child2>
		</template>

		<script>
			method: { 
				_drop(target) {   // 接收
	               this.$nextTick(() => {
	                    this.$refs.child2.drop(target)  // 又派发给 child2
	                });
            	},
				
			}
		</script>

	
	child2.vue 

	<script>
		methods: {
			drop(target) {   // 接收
				console.log(target);
			}
		}
	</script>


## 父组件 使用 子组件方法 

	
	parent.vue 
	
	this.$refs.child.show();



## getBoundingClientRect




## better-scroll 
	

	refresh();  // 重新计算 better-scroll，当 DOM 结构发生变化的时候务必要调用确保滚动的效果正常。