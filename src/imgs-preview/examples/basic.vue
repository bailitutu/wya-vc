<template>
	<div>
		<!-- 自定义 renderRow -->
		<vc-imgs-preview :data-source="dataSource" :render-row="renderRow" />
		
		<!-- 自定义 renderRow -->
		<vc-imgs-preview :data-source="dataSource">
			<template #row="it">
				<img 
					:src="it.src" 
					:key="it.index" 
					:style="{ width: '100px', height: '100px', borderRadius: '20px' }"
				>
			</template>
		</vc-imgs-preview>

		<!-- 自定义 operate -->
		<vc-imgs-preview :data-source="dataSource">
			<template #operate="it">
				<div @click="it.show($event, it.index)">{{ it.index }}</div>
			</template>
		</vc-imgs-preview>
		<span @click="handleClick">自定义预览</span>
	</div>
</template>
<script>
import ImgsPreview from '..';

export default {
	name: "vc-imgs-preview-basic",
	components: {
		'vc-imgs-preview': ImgsPreview
	},
	data() {
		return {
			dataSource: [
				{
					src: 'https://oss.ruishan666.com/image/xcx/180228/803943951788/裤子.png',
					title: 'Image 1',
					w: 1200,
					h: 900
				},
				{
					src: 'https://oss.ruishan666.com/image/xcx/180313/942990884682/10053600,2880,1800.jpg',
					title: 'Image 2',
					w: 1200,
					h: 900
				},
				{
					src: 'https://oss.ruishan666.com/image/xcx/180313/942990767112/10049533,2880,1800.jpg',
					title: 'Image 2',
					w: 1200,
					h: 900
				},
				{
					src: 'https://oss.ruishan666.com/image/xcx/180228/803943510611/衣服-01.png',
					title: 'Image 2',
					w: 1200,
					h: 900
				},
				{
					src: 'https://oss.ruishan666.com/image/xcx/180313/942996157518/10053669,2880,1800.jpg',
					title: 'Image 2',
					w: 1200,
					h: 900
				}

			],
			opts: {
				closeOnScroll: false
			}
		};
	},
	computed: {
		
	},
	methods: {
		renderRow(h, props, parent) {
			const { src, index } = props; 
			return (
				<img 
					src={src} 
					key={index} 
					style={{ width: '100px', height: '100px', borderRadius: '50px' }} 
				/>
			);
		},
		handleClick(e) {
			let pos = {};
			try {
				const target = e.target; // 先得到pos, 否则getThumbBoundsFn再计划，target已变化（比如弹窗transition的影响）
				const pageYScroll = window.pageYOffset || document.documentElement.scrollTop;
				const rect = target.getBoundingClientRect();

				pos = { x: rect.left, y: rect.top + pageYScroll, w: rect.width };

			} catch (e) {
				console.log(e);
			}
			ImgsPreview.open({
				visible: true,
				dataSource: this.dataSource,
				opts: {
					index: 2,
					history: false,
					getThumbBoundsFn: (index) => pos
				}
			});
		}
	}
};
</script>
