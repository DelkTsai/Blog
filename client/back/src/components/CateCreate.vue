<template>
      <div class="fk-content-wrap">
        <div class="manage-container">
          <form
            model={tagInfo}
            class="tag-create clearfix"
            onsubmit="return false"
          >
            <div class="form-group">
                <label class="control-label col-xs-1">类型名称</label>
                <div class="col-xs-4">
                    <input
                        name="name"
                        class="form-control"
                        type="text"
                        label="标签名称"
                        v-model="name"
                        validate="required"
                    />
                </div>  
            </div>
            <div class="form-group">
                <label class="control-label col-xs-1">缩略名</label>
                <div class="col-xs-4">
                    <input
                        name="pathname"
                        type="text"
                        label="缩略名"
                        class="form-control"
                        validate="required"
                    />
                </div>  
            </div>
            <div class="form-group col-xs-12">
              <button type="submit" @click="submitCate" class="btn btn-primary">{{ isSubmitting ? '提交中...' : '提交'}}</button>
            </div>
          </form>
        </div>
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
      name: '',
      id: '',
      isSubmitting: false,
    }
  },
  route: {
    data({ to }){
      if (typeof to.params.id == 'undefined'){
        return;
      }

      this.id = to.params.id;

      Api.fetchCateById(this.id).then(result=>{
        this.name = result.name;
      })
    }

  },
  methods: {
    submitCate() {
      this.isSubmitting = true;

      this.tipInfo = "已成功提交";
      this.tipType = 'success'; 
      this.shouldTipShow = true;
      setTimeout(()=>{
          this.shouldTipShow = false;
          this.$router.go({ path: '/cate/list' });
      }, 2000);

      if (this.id == '')
        Api.newCate(this.name).then(body=>{
          this.isSubmitting = false;
        })
      else
        Api.patchCate(this.id ,{ name: this.name }).then(body=>{
          this.isSubmitting = false;
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limi t CSS to this component only -->
<style scoped>
    h1 {
        color: #42b983;
    }
</style>