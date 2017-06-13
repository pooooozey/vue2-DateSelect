<template>
  <div>
  	<date-select @setBirthday="setBirthday" :defyear="-1" :defmonth="-1" :defday="-1"></date-select>
  	
  </div>
</template>

<script>
import DateSelect from './DateSelect.vue'

export default {
	data(){
		return {
				birthday : ''
		}
	},
  components : {
    DateSelect
  },
  methods : {
  	setBirthday(opt){
  		this.birthday = opt.day
  	}

  }
}

</script>

<style scoped>

</style>
