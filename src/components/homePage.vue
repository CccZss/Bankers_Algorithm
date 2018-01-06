<template>
	<section>
		<h1>Bankers Algorithm ---- By Czs</h1>
		<div  class="step step-one">
			<!-- <div  class="step step-one" v-show="currentStep==2"> -->
			<div>
				<h2 class="center">设置初始值</h2>
				<div>
					<span>资源数</span> 
					<InputNumber :min="1" :max="8" v-model="resourceNum"></InputNumber>
					<span class="ml10">进程数</span>
					<InputNumber :min="1" :max="8" v-model="processNum"></InputNumber>
				</div>
				<br>
				<div>
					<span>总的资源数量</span> 
					<span class="inline-block ml10" :key="'allResouce-' + index" v-for="index in allResouce.resource.length">
						{{String.fromCharCode(index - 1 + 65)}}
						<InputNumber class="w55" :min="1" v-model="allResouce.resource[index-1]"></InputNumber>
					</span>
					<Button class="ml10" type="primary" @click="createRandomData">随机数据</Button>
					<span class="ml10">成功率：</span> 
					<Slider class="success-rate ml5" :min="20" :max="80" v-model="successRate"></Slider>
				</div>
				<ul class="table table1">
					<li>
						<span class="table-left-item">Process</span>
						<span class="process-item-wrap">
							<p>Max</p>
							<span class="w55" v-for="index in resourceNum" :key="'max-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Need</p>
							<span class="w55" v-for="index in resourceNum" :key="'need-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Allocation</p>
							<span class="w55" v-for="index in resourceNum" :key="'allocation-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
					</li>
					<li :key="'process-' + index" v-for="(item, index) in processList">
						<span class="table-left-item">{{'P'+ item.id}}</span>
						<span class="process-item-wrap">
							<!-- max:  -->
							<span v-for="index2 in resourceNum" :key="'P' + index + '-max-' + index2">
								<InputNumber class="w55" :min="0" v-model="item.max[index2-1]"></InputNumber>
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Need:  -->
							<span v-for="index2 in resourceNum" :key="'P-' + index + '-need-' + index2">
								<InputNumber class="w55" :min="0" v-model="item.need[index2-1]"></InputNumber>
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Allocation:  -->
							<span v-for="index2 in resourceNum" :key="'P' + index + '-allocation-' + index2">
								<InputNumber class="w55" :min="0" v-model="item.allocation[index2-1]"></InputNumber>
							</span>
						</span>
					</li>
				</ul>
			</div>

			<div>
				<ul class="table table2">
					<li>
						<span class="table-left-item tow-row-tb">Process</span>
						<span class="process-item-wrap">
							<p>Max</p>
							<span class="w34" v-for="index in resourceNum" :key="'max-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Need</p>
							<span class="w34" v-for="index in resourceNum" :key="'need-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Allocation</p>
							<span class="w34" v-for="index in resourceNum" :key="'allocation-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Available</p>
							<span class="w34" v-for="index in resourceNum" :key="'allocation-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
					</li>
					<li :key="'process-' + index" v-for="(item, index) in processList">
						<span class="table-left-item">{{'P'+ item.id}}</span>
						<span class="process-item-wrap">
							<!-- max:  -->
							<span class="w34" v-for="index2 in resourceNum" :key="'P' + index + '-max-' + index2">
								{{item.max[index2-1]}}
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Need:  -->
							<span class="w34" v-for="index2 in resourceNum" :key="'P-' + index + '-need-' + index2">
								{{item.need[index2-1]}}
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Allocation:  -->
							<span class="w34" v-for="index2 in resourceNum" :key="'P' + index + '-allocation-' + index2">
								{{item.allocation[index2-1]}}
							</span>
						</span>
						<span v-if="index==0" class="process-item-wrap">
							<!-- Allocation:  -->
							<span class="w34" v-for="(item2, index2) in allResouce.resource" :key="'P-' + index + '-workAllocation'+ index2">
								{{item2}}
							</span>
						</span>
					</li>
				</ul>
			</div>
			<Button class="comfirm-bt" type="primary" @click="firstCheckSafe">确定</Button>
		</div>
		<div class="step">
			<h2 class="center">进行检查</h2>
			<div class="mt20">
				<span>添加进程请求:</span>
				<div class="process-request-box ml10">
					<Select v-model="selectProcess" class="w55">
						<Option v-for="item in processList" :value="item.id" :key="item.id">{{ 'P' + item.id }}</Option>
					</Select>
					<span class="w70 ml10" v-for="item in resourceNum" :key="'require-' + item">
						<span>{{String.fromCharCode(item - 1 + 65)}}</span> 
						<InputNumber class="w55" :min="0" v-model="processRequest[item-1]"></InputNumber>
					</span>
				</div>
				<Button class="ml10" type="primary" @click="requireRecourse">require</Button>
			</div>
			<div class="mt20 block">
				<span>已请求队列: </span>
				<span class="ml10 inline-block">
					<span v-for="(item, index) in handelList" :key="'handel' + index">
						<span>P{{item.processId}}
							(
							<span v-for="(item2, index) in resourceNum" :key="'require-' + item2 +'-'+ index">
								{{item.needResource[index]}}
								<span v-if="index !== resourceNum-1"> , </span>
							</span>
							) 
							<span v-if="index !== (handelList.length - 1)"> ---> </span>
						</span>
					</span>
				</span>
			</div>
			<div>
				<ul class="table table2">
					<li>
						<span class="table-left-item tow-row-tb">Process</span>
						<span class="process-item-wrap">
							<p>Work</p>
							<span class="w34" v-for="index in checkAllResourceNum" :key="'max-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Need</p>
							<span class="w34" v-for="index in checkAllResourceNum" :key="'need-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Allocation</p>
							<span class="w34" v-for="index in checkAllResourceNum" :key="'allocation-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="process-item-wrap">
							<p>Work + Allocation</p>
							<span class="w34" v-for="index in checkAllResourceNum" :key="'allocation-' + index">
								{{String.fromCharCode(index - 1 + 65)}}
							</span>
						</span>
						<span class="table-left-item tow-row-tb">Finish</span>
					</li>
					<li :key="'process-' + index" v-for="(item, index) in safeList">
						<span class="table-left-item">{{'P'+ tocheckProcessList[item].id}}</span>
						<span class="process-item-wrap">
							<!-- max:  -->
							<span class="w34" v-for="index2 in checkAllResourceNum" :key="'P' + index + '-work-' + index2">
								{{ tocheckProcessList[item].work[index2-1]}}
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Need:  -->
							<span class="w34" v-for="index2 in checkAllResourceNum" :key="'P-' + index + '-need-' + index2">
								{{ tocheckProcessList[item].need[index2-1]}}
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Allocation:  -->
							<span class="w34" v-for="index2 in checkAllResourceNum" :key="'P' + index + '-allocation-' + index2">
								{{ tocheckProcessList[item].allocation[index2-1]}}
							</span>
						</span>
						<span class="process-item-wrap">
							<!-- Allocation:  -->
							<span class="w34" v-for="index2 in checkAllResourceNum" :key="'P' + index + '-work-allocation-' + index2">
								{{ tocheckProcessList[item].work[index2-1] + tocheckProcessList[item].allocation[index2-1]}}
							</span>
						</span>
						<span class="process-item-wrap w60">
							<!-- Finish:  -->
							true
						</span>
					</li>
				</ul>
			</div>
		</div>
		<!-- <div class="step-bt">
			<Button type="primary" v-show="currentStep > 1" @click="toLastStep">Last</Button>
			<Button type="primary" v-show="currentStep < stepNum" @click="toNextStep">Next</Button>
		</div> -->
	</section>
</template>

<script>
import { Button, InputNumber, Select, Option, Slider } from 'iview';
export default {
	data() {
		return {
			stepNum: 3,
			currentStep: 1,

			processNum: 1,  // 进程数量
			resourceNum: 1,  // 进程竞争的资源数,
			checkAllResourceNum: 1,
			successRate: 50,

			selectProcess: 0,
			processRequest: [],
			safeList: [],
			afterRequestAllResource: [],

			allResouce: {
				resource: [],
			},
			processList: [],   // 用于初始化设置进程需要资源信息
			tocheckProcessList: [],  // copy 初始化的信息用于检查时使用
			handelList: [],  // {processId: ,needResource: }
			blockHandelList: [],
		}
	},
	components: {
		Button,
		InputNumber,
		Select,
		Option,
		Slider
	},
	methods: {
		/* toNextStep() {
			if(this.currentStep >= this.stepNum) {
				return
			}else {
				this.currentStep++;
			}
		},
		toLastStep() {
			if(this.currentStep <= 1) {
				return
			}else {
				this.currentStep--;
			}
		}, */
		changeResourceNum(newVal, oldVal) {
			if(newVal < 0 || oldVal === newVal){
				return;
			}

			this.handelList = [];

			if(this.allResouce.resource.length === 0) {  // 一开始还没有初始化时，初始化各个数组变量

				// 设置总的资源的初始值
				this.allResouce = {
					resource: Array(newVal).fill(0)
				}

				this.processRequest =  Array(newVal).fill(0);
				
				// 设置各个进程资源的初始值
				this.processList = [];
				for(var i=0; i<this.processNum; i++) {
					this.processList.push({
						id: i,
						max: Array(newVal).fill(0),
						need: Array(newVal).fill(0),
						allocation: Array(newVal).fill(0),
						work: Array(newVal).fill(0),
						finish: false
					})
				}

			}else{  // 已初始化过，只需修改对应数值即可

				var dif = newVal - oldVal;
				if(dif > 0) {  
					// 资源总数数量部分
					for(var i=0; i<dif; i++) {
						this.allResouce.resource.push(0);
						this.processRequest.push(0);
						 
					}
					// 每个进程部分
					for(var i=0; i<this.processNum; i++) {
						for(var j=0; j<dif; j++) {
							this.processList[i].max.push(0);
							this.processList[i].need.push(0);
							this.processList[i].allocation.push(0);
						}						
					}
				}else if(dif < 0) { 
					this.allResouce.resource.splice(dif);
					this.processRequest.splice(dif);
					for(var i=0; i<this.processNum; i++) {
						this.processList[i].max.splice(dif);
						this.processList[i].need.splice(dif);
						this.processList[i].allocation.splice(dif);
					}
				}
			}

			
		},
		changeProcessNum(newVal, oldVal) {
			var dif = newVal - oldVal;
			if(dif > 0) {
				var currentProcessNum = this.processList.length;
				for(var i=0; i<dif; i++) {
					this.processList.push({
						id: i + currentProcessNum,
						max: Array(this.resourceNum).fill(0),
						need: Array(this.resourceNum).fill(0),
						allocation: Array(this.resourceNum).fill(0),
						work: Array(newVal).fill(0),
						finish: false
					})
				}
			}else if(dif < 0) {
				this.processList.splice(dif)
			}
		},
		createRandomData() {
			var processWordMax = 10;
			var processWordMin = 0;
			var allResouceMax = this.successRate / 100 * 2 * processWordMax;
			var allResouceMin = this.successRate / 100 * 2 * processWordMin;

			this.allResouce.resource = this.allResouce.resource.map(() => {
				return parseInt(Math.random()*(allResouceMax -  allResouceMin) +  allResouceMin,10);
			})

			this.processList.forEach( item => {
				for(var i=0; i<item.need.length; i++){
					var workNum = parseInt( Math.random()*(processWordMax -  processWordMin) +  processWordMin);
					var needNum = parseInt(Math.random() * workNum);
					item.max[i] = workNum;
					item.need[i] = needNum;
					item.allocation[i] = workNum - needNum;
				}
			})
		},
		requireRecourse() {
			var selectProcess = this.selectProcess;
			var processRequest = [].concat(this.processRequest);
			if(processRequest.every((item)=>{return item==0})) {
				alert('请求的资源不能全为零！')
				return;
			}

			if(this.tocheckProcessList.length === 0) {
				this.tocheckProcessList = JSON.parse(JSON.stringify(this.processList));
			}
			var processNeed = this.tocheckProcessList[selectProcess].need.concat();
			var allocation = this.tocheckProcessList[selectProcess].allocation.concat()
			var allResouce = this.afterRequestAllResource.concat();

			var newProcessNeed = [];
			var newAllResouce = [];
			var newAllocation = [];
			var oldtoCheckProcessList = JSON.parse(JSON.stringify(this.tocheckProcessList));
			var oldSafeList = JSON.parse(JSON.stringify(this.safeList));

			var result = processRequest.every((item, index) => {
				var r = item <= processNeed[index] && item <= allResouce[index]
				newProcessNeed[index] = processNeed[index] - item;
				newAllResouce[index] = allResouce[index] - item;
				newAllocation[index] = allocation[index] + item;
				return r
			})

			if(result) {
				this.tocheckProcessList[selectProcess].need = newProcessNeed;
				this.tocheckProcessList[selectProcess].allocation = newAllocation;
				var r = this.check(this.tocheckProcessList, newAllResouce);
				if(r) {
					this.handelList.push({
						processId: selectProcess,
						needResource: processRequest
					})
				}else {
					this.tocheckProcessList = oldtoCheckProcessList;
					this.safeList = oldSafeList;
				}
			}else {
				alert('请求资源超过当前总资源或超过该进程需要的资源数，请求失败！')
			}

		},
		firstCheckSafe() {
			this.handelList = [];
			this.checkAllResourceNum = this.resourceNum;
			this.tocheckProcessList = JSON.parse(JSON.stringify(this.processList));
			this.afterRequestAllResource = this.allResouce.resource.concat();
			this.check(this.tocheckProcessList, this.allResouce.resource.concat());
		},
		/**
		 * 查看当前的进程信息及系统可提供的资源十分能安全执行所有进程
		 */
		check(processList, allResouce) {
			var processNum = this.processNum;
			var arr = Array.apply(null, Array(processNum)).map((item, index) => {
				return index;
			})

			// 递归得到进程所有的可能组合
			var allcombination = this.combination(arr);
			this.safeList = [];

			// 看那个进程顺序组合能满足安全要求
			allcombination.some((arr, index) => {
				var result = arr.every((processId, index2) => {
					if(index2 === 0) {
						processList[processId].work = allResouce;
					}
					var need = processList[processId].need;
					var nextWork = [];
					var isSave = need.every((item, index3) => {
						nextWork[index3] = processList[processId].work[index3] + processList[processId].allocation[index3];
						return processList[processId].work[index3] >= item
					})
					if(isSave) {
						var nextprocessId = arr[index2+1];
						if(nextprocessId != undefined) {
							processList[nextprocessId].work = nextWork;
						}
					}else {
						// 某个进程不安全
						return false;
					}
					return true;
				})
				if(result === true) {
					this.safeList = arr
				}
				return result;
			})

			if(this.safeList.length === 0 ) {
				alert('not safe')
				this.safeList = [];
				return false;
			}else{
				alert('安全顺序为：'+ this.safeList)
				this.afterRequestAllResource = allResouce;
				return true;
			}
		},
		/**
		 * 对输入的数组尝试所有的组合情况并输出
		 */
		combination(arr) {
			var result = [];
			if(arr.length > 1) {
				arr.forEach((item, index) => {
					var arr2 = arr.concat([])
					arr2.splice(index, 1)
					this.combination(arr2).forEach(item2 => {
						result.push([].concat(item, item2))
					})
				})

			} else if (arr.length == 1) {
				return [arr];
			}
			return result;
		}
	},
	watch: {
		resourceNum (newVal, oldVal) {
			this.changeResourceNum(newVal, oldVal)
		},
		processNum (newVal, oldVal) {
			this.changeProcessNum(newVal, oldVal)
		},
	},
	mounted() {
		this.changeResourceNum(this.resourceNum, 0);
	}
}
</script>X

<style scoped>
	section {
		position: relative;
		display: inline-block;
		width: 90%;
	}
	h1 {
		margin-bottom: 20px;
	}
	.step {
		width: 100%;
		min-height: 200px;
		padding: 20px 20px 40px;
		margin: auto;
		margin-bottom: 20px;
		text-align: left;
		box-shadow:  3px 3px 10px 2px rgb(95, 75, 75);
		background-color: #e9f1fc;

	}
	.step .table li {
		display: block;
		border-top: 1px solid black;
	}
	.step .table{
		margin-top: 20px;
		display: inline-block;
		border: 1px solid;
	}
	.table1 .table-left-item {
		display: inline-block;
		width: 60px;
		text-align: center;
		padding: 8px 0;
	}
	.table1 .process-item-wrap {
		display: inline-block;
		min-width: 190px;
		padding: 8px 10px;
		text-align: center;
		border-left: 1px solid black;
		box-sizing: border-box;
	}
	.table2 .table-left-item {
		display: inline-block;
		width: 60px;
		text-align: center;
		border-right: 1px solid black;
		padding: 8px 0;
	}
	.table2 .process-item-wrap {
		display: inline-block;
		min-width: 190px;
		padding: 8px 10px;
		text-align: center;
		border-right: 1px solid black;
		box-sizing: border-box;
	}
	.step .tow-row-tb {
		line-height: 53px;
		padding: 0;
		vertical-align: bottom;
	}
	.process-item-wrap p{
		border-bottom: 1px solid black;
		margin: 0 -11px;
	}
	.step-bt {
		position: absolute;
		bottom: 5px;
		right: 5px;
	}
	.process-request-box {
		display: inline-block;
		padding: 3px 5px;
		border-radius: 5px;
		border: 1px solid #bbbbb8;
	}
	.comfirm-bt{
		margin-top: 30px;
		width: 100%;
	}
	.success-rate {
		width: 150px;
		display: inline-block;
		vertical-align: bottom;
	}
	
	.center {
		text-align: center;
	}
	.w55 {
		display: inline-block;
		width: 55px;
	}
	.w34 {
		display: inline-block;
		width: 34px;
	}
	section .step .w60 {
		min-width: 60px;
		width: 60px;
		display: inline-block;
	}
	.w70 {
		display: inline-block;
		width: 70px;
	}
	.ml10 {
		display: inline-block;
		margin-left: 10px;
	}
	.ml5 {
		display: inline-block;
		margin-left: 5px;
	}
	.mt20 {
		display: inline-block;
		margin-top: 20px;
	}
	.block {
		display: block;
	}
</style>
