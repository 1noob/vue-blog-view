<template>
	<div>
    <div class="ui top attached segment">
      <div class="ui link three doubling cards">
        <a :href="item.website" target="_blank" rel="external nofollow noopener" class="card" :style="randomRGB()"
           v-for="(item,index) in friendList" :key="index" @click="addViews(item.nickname)">
          <div class="image">
            <img :src="item.avatar" onerror="this.src = '/img/error.png'">
          </div>
          <div class="content">
            <div class="header">{{ item.nickname }}</div>
            <div class="description">{{ item.description }}</div>
          </div>
        </a>
      </div>
      <br/>
      <div class="typo content" v-viewer v-html="info.content"></div>
    </div>
	</div>
</template>

<script>
	import {getData, addViewsByNickname} from "@/api/friend";
	import CommentList from "@/components/comment/CommentList";

	export default {
		name: "Friends",
		components: {CommentList},
		data() {
			return {
				friendList: [],
				bgColor: [
					'#1abc9c', '#2ecc71', '#3498db', '#9b59b6',
					'#34495e', '#f1c40f', '#e67e22', '#e74c3c',
					'#ee5a24', '#9980fa', '#8c7ae6', '#f79f1f'
				],
				info: {
					content: '',
					commentEnabled: false
				},
			}
		},
		created() {
			this.getData()
		},
		methods: {
			getData() {
				getData().then(res => {
					if (res.code === 200) {
						this.friendList = res.data.friendList
						this.info = res.data.friendInfo
					} else {
						this.msgError(res.msg)
					}
				}).catch(() => {
					this.msgError("请求失败")
				})
			},
			addViews(nickname) {
				addViewsByNickname(nickname)
			},
			randomRGB() {
				const index = Math.floor((Math.random() * this.bgColor.length))
				return {backgroundColor: this.bgColor[index]}
			}
		}
	}
</script>

<style scoped>
	.card .image {
		width: 70px !important;
		margin: 10px auto 0;
		background: unset !important;
	}

	.card .image img {
		border-radius: 0;
		width: 70px !important;
		height: 70px !important;
	}

	.card .content {
		text-align: center;
		padding: 10px 14px !important;
		border-top: 0 !important;
	}

	.card .content * {
		color: #fff !important;
	}

	.card .content .header {
		font-size: 16px !important;
	}

	.card .content .description {
		font-size: 12px !important;
	}

	.card .content .description {
		margin-top: 5px !important;
		margin-bottom: 7px;
	}
</style>
