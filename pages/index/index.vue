<template>
	<view class="page">
		<WaterfallsFlow :wfList='list' @itemTap="itemTap" />
	</view>
</template>

<script>
	import WaterfallsFlow from '../../components/WaterfallsFlow/WaterfallsFlow.vue'
	
	export default {
		components:{
			WaterfallsFlow
		},
		data() {
			return {
				list:[],
				requiredData:[
					{
						image:'https://cdn.pixabay.com/photo/2015/10/21/03/44/beach-998788_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2015/10/12/02-06-28-605_250x250.jpg',
						nickName:'rFonz'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/08/22/10/couple-5276245_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2020/04/02/05-34-12-636_250x250.jpg',
						nickName:'VlacK_HammeR'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/11/21/25/imp-5288348_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2020/06/10/08-34-47-445_250x250.jpg',
						nickName:'Alexas_Fotos'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/10/01/32/cumulonimbus-5280578_960_720.jpg',
						avatar:'https://pixabay.com/static/img/profile_image_dummy.svg',
						nickName:'phil454'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/06/04/16/table-5265190_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2020/06/13/19-29-33-253_250x250.jpg',
						nickName:'NickyPe'
					},
					{
						image:'https://cdn.pixabay.com/photo/2016/03/26/13/09/notebook-1280538_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2017/08/03/10-16-32-389_250x250.png',
						nickName:'freephotocc'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/08/05/28/vortex-5273023_960_720.png',
						avatar:'https://cdn.pixabay.com/user/2015/12/02/23-35-18-266_250x250.png',
						nickName:'GDJ'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/03/16/30/wave-5255593_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2020/05/23/12-55-39-456_250x250.jpg',
						nickName:'pasja1000'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/05/28/08/00/girl-5230306_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2020/01/29/07-13-31-807_250x250.jpg',
						nickName:'Syaibatulhamdi'
					},
					{
						image:'https://cdn.pixabay.com/photo/2020/06/10/09/19/cake-5281948_960_720.jpg',
						avatar:'https://cdn.pixabay.com/user/2018/01/25/18-40-42-65_250x250.jpg',
						nickName:'matthiasboeckel'
					}
				]
			}
		},
		onLoad() {
			// 模拟请求数据
			setTimeout(()=>{
				this.list = this.requiredData;
			},1000)
		},
		onReachBottom(){
			// 模拟触底刷新
			setTimeout(()=>{
				this.requiredData.push(...this.list);
			},1000)
		},
		onPullDownRefresh(){
			// 模拟上拉刷新
			setTimeout(()=>{
				const newList = this.requiredData.reverse();
				this.list = newList;
				uni.stopPullDownRefresh();
			},1000)
		},
		methods:{
			itemTap(item){
				console.log(item);
			}
		}
		
	}
</script>

<style lang="scss" scoped>
	.page{
		min-height: 100vh;
		background: #eee;
	}
</style>
