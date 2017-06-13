<template>
  <div>
  	<select @change="changeDate" v-model="curYear">
			<option value="-1">{{languageTxt.year}}</option>
			<option v-for="item in year" :value="item">{{item}}</option>
		</select>
		<select @change="changeDate" v-model="curMonth">
			<option value="-1">{{languageTxt.month}}</option>
			<option v-for="item in month" :value="item">{{item+1}}</option>
		</select>
		<select @change="getCurDate" v-model="curDay">
			<option value="-1">{{languageTxt.day}}</option>
			<option v-for="item in day" :value="item">{{item}}</option>
		</select>
		{{setCurDate}}
  </div>
</template>

<script>
export default {
	props : ['defyear','defmonth','defday'],
	data(){
		return {
			year : [],
			month : [],
			day : [],
			curYear : -1,
			curMonth : -1,
			curDay : -1
		}
	},
	computed: {
    languageTxt () {
      return this.$store.state.language.currentLanguage.txt.Login
    },
    setCurDate(){
    	this.curYear = this.defyear
    }
  },
	mounted(){
		this.createYear()
		this.createMonth()
	},
	methods:{
		isLeapYear(){
		  return (this.curYear % 4 == 0 || (this.curYear % 100 == 0 && this.curYear % 400 == 0))
		},
		createYear(){
			let arr = []
			for(let i=new Date().getFullYear();i>=1900 ;i--){
				arr.push(i)
			}
			this.year = arr
		},
		createMonth(){
			let arr = []
			for(let i=0;i<12 ;i++){
				arr.push(i)
			}
			this.month = arr

			if(typeof this.defmonth==='number'){
				this.curMonth = this.defmonth
				this.changeDate()
			}
			

		},
		createDay(dayNum){
			let arr = []
			for(let i=0;i<dayNum ;i++){
				arr.push(i+1)
			}
			this.day = arr

    	this.curDay = this.defday

		},
		changeDate(){
			let monthDays = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
			this.curDay = -1

			if(this.curYear>=0&&this.curMonth>=0){
				let dayNum = monthDays[this.curMonth]
				if (this.curMonth == 1 && this.isLeapYear()) {  
		      dayNum++
		    }
		    this.createDay(dayNum)
			}

		},
		getCurDate(){
			//加0
			
			let mon = (this.curMonth<10)?('0'+(this.curMonth+1)):this.curMonth+1
			let day = (this.curDay<10)?('0'+this.curDay):this.curDay

			console.log(mon,day)

			if(this.curYear>=0&&this.curMonth>=0&&this.curDay>=0){
				this.$emit("setBirthday",{
					day : this.curYear+'-'+mon+'-'+day
				})
			}
			
		}
	}
  
}

</script>

<style scoped>
.register select{
	width:65px;
	height:38px;
	margin:0 25px 0 0;
  border:1px solid #dedede;
	font-size:14px;
	background:#fff;
}
.base select {
  border: 1px solid #F0f0f0;
  outline: none;
  background: #F3F6F9;
  height: 30px;
  width: 86px;
  padding: 5px 0;
  vertical-align: middle;
  margin-right: 10px;
}
</style>
