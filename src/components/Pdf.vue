<template>
	<div class="container-fluid">
		<div id="PdfHeader">
			<div class="mainbox">
				<div class="title withoutline">
					<i class="fas fa-file-pdf"></i> Vue Js PDF: <span class="text-danger">View PDF documents easily and creatively.</span>
					<span class="right" id="PageInfo" v-if="!error">Page {{current}}/{{totalPage}}</span>
					<span class="right mr-3" v-if="!error">
						<button class="btn btn-sm btn-success mr-2" id="prev-page" @click="RenderPage(current-1)" :disabled="current <= 1"><i class="fas fa-arrow-circle-left mr-1"></i> Prev Page</button>
						<button class="btn btn-sm btn-success" id="next-page" @click="RenderPage(current+1)" :disabled="totalPage<=current">Next Page <i class="fas fa-arrow-circle-right ml-1"></i></button>
					</span>
					<span class="right mr-3" v-if="!error">
						<button class="btn btn-sm btn-dark text-white mr-2"><i class="fas fa-search"></i> x{{(scale).toFixed(2)}}</button>
						<button class="btn btn-sm btn-danger mr-2" id="prev-page" @click="ScaleChange('-')" :disabled="scale <= 0.4"><i class="fas fa-minus-square mr-1"></i> %10</button>
						<button class="btn btn-sm btn-danger" id="next-page" @click="ScaleChange('+')" :disabled="scale>=4"><i class="fas fa-plus-square mr-1"></i> %10</button>
					</span>
				</div>
			</div>
		</div>

		<div class="mainbox" id="ContentArea">
			<div class="title"><i class="fas fa-file-pdf"></i> {{title}}</div>
			<div class="content scroll text-center" style="">
				<div class="alert alert-danger text-left" v-if="error">Something went wrong, please try again later. Or you can check the file name of your PDF. </div>
				<canvas id="PdfRender" v-else></canvas>
			</div>
		</div>
	</div>
</template>
<style scoped>
</style>
<script>
export default {
	data(){
		return{
			pdfDoc: null,
			current: 1,
			totalPage: null,
			isRendering: false,
			isPending: null,
			canvas: null,
			ctx: null,
			error: false
		}
	},
	props: {
		file: {type: String, required: true},
		scale: {type: Number, default:3},
		title: {type: String}
	},
	methods:{
		ScaleChange(val){
			if(val=="-"){this.scale /= 1.1}
			if(val=="+"){this.scale *= 1.1}
			this.RenderPage(this.current)
		},
		RenderPage(current){
			this.isRendering = true
			this.current = current
			this.pdfDoc.getPage(this.current).then(page => {
				let scale = this.scale
				const viewport = page.getViewport({ scale });
				this.canvas.height = viewport.height;
				document.getElementById("PdfRender").style.height = parseInt(viewport.height) + 100 + 'px'
				this.canvas.width = viewport.width;

				const renderCtx = {canvasContext: this.ctx, viewport}
				page.render(renderCtx).promise.then(() => {
					this.isRendering = false
					if (this.isPending !== null) {
						this.RenderPage(this.isPending)
						this.isPending = null
					}
				});
			});
		}
	},
	mounted(){
		this.canvas = document.getElementById("PdfRender")
		this.ctx = this.canvas.getContext('2d')
		window.pdfjsLib
			.getDocument(this.file)
				.promise.then(pdfDoc_ => {
					this.pdfDoc = pdfDoc_
					this.totalPage = this.pdfDoc.numPages
					this.RenderPage(this.current);
				})
				.catch(err => this.error = err.message)
	}
}
</script>