<template>
  <div class="fk-content-wrap">
    <div class="manage-container" >
      <h3 style="margin-bottom:20px;">网站统计代码</h3>
      <form onsubmit="return false" class="clearfix options-general">
        <div class="form-group">
          <div class="form-group"><textarea type="textarea" name="analyze_code" class="form-control form-control" v-model="analyze_code" style="height:240px;"></textarea></div>
          <p>直接贴入百度统计或者 Google 统计代码</p>
        </div><button type="submit" @click="submit" class="btn btn-primary">提交</button></form>
    </div>
</template>

<script>
/* eslint-disable */
import Top from './Top';
import Api from '../store/api';

export default {

  components: {
    Top,
  },
  props:{
    shouldTipShow: Boolean,
    tipType: String,
    tipInfo: String,
    currentRoute: String,
  },
  data () {
    return {
      option: {},
      analyze_code: '',
    }
  },
  methods: {
    getOption(){
      Api.fetchOption().then(result=>{
        let obj = {};
        result.forEach(value=>{
          obj[value.key] = value;
        })
        this.option = obj;
        this.analyze_code = obj['analyze_code'].value;
      })
    },
    submit(){
      this.tipInfo = "统计代码已更新";
      this.tipType = 'success'; 
      this.shouldTipShow = true;
      setTimeout(()=>{
          this.shouldTipShow = false;
      }, 2000);

      Api.patchOption(this.option['analyze_code']._id,{ value : this.analyze_code }).then(result=>{

      })

    }
  },
  ready () {
    this.getOption();
  }
}
</script>

<!-- Add "scoped" attribute to limi t CSS to this component only -->
<style scoped>
  h1 {
    color: #42b983;
  }
</style>