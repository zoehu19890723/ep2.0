<template>
  <div class="page">
    <NavBar :headerInfo="head"></NavBar>
    <v-content className="page-content page-background">
      <InputText ref="newText" :isPwd="false" :dataId="field_code" :dataPlaceholder="placeholder" :defaultV="defaultV" class="specInputTxt border-color"></InputText>
      <SubmitBtn :btnText="btn.text" :submitAction="updateField" :clx="btn.clx"></SubmitBtn>
    </v-content>
  </div>
</template>

<script>
  import VContent from '../../components/Content';
  import InputText from '../../components/InputText';
  import SubmitBtn from '../../components/SubmitBtn';
  import NavBar from '../../components/NavBar';

  export default {
    created:function(){
      this.head.title = this.$t('edit') + this.$route.query.title || '';
    },
    data() {
      return {
        head:{
          backShow:true,
          rightShow:false,
          rightInfo:'',
          show:true,
          title:''
        },
        btn :{
          text : this.$t('commitButton'),
          clx : 'weui_btn bg-red',
        }
      }
    },
    computed:{
      field_code(){
        return this.$route.query.field_code;
      },
      groupId(){
        return this.$route.query.groupId || null;
      },
      placeholder(){
        return this.$t('plsInput') + this.$route.query.title || '';
      },
      defaultV(){
        return this.$route.query.value;
      },
      nullable(){
        return this.$route.query.nullable;
      },
      title(){
        return this.$t('edit') + this.$route.query.title || ''
      }
    },
    methods:{
      updateField(){
        let value = this.$refs.newText.dataValue;
        if(this.nullable === false && (value === null || value.trim().length === 0)){
          f7.alert('',this.$t('cannot-null'));
          return;
        }
        this.$store.dispatch('updateField',{
          field_code: this.field_code,
          groupId : this.groupId,
          value : value
        }).then((response)=>{
          if(response.body.status === 1){
            let me = this;
            f7.alert('',me.$t('modifySucceed'),()=>{
              this.$router.back();
            })
          }else{
            f7.alert('',response.body.message);
          }
        },(error)=>{
          f7.alert(error);
        });
      }
    },

    components: {
      InputText,
      SubmitBtn,
      VContent,
      NavBar
    },

    locales: {
      zh: {
        'edit' : '编辑',
        'commitButton' : '提交',
        'cannot-null' : '不能为空',
        'modifySucceed' : '修改成功',
        'plsInput' : '请输入',
        'submitting':'提交中...',
      },

      en: {
        'edit' : 'Edit ',
        'commitButton' : 'Submit',
        'cannot-null' : 'can not be null',
        'modifySucceed' : 'Modify Succeed!',
        'plsInput' : 'Please enter',
        'submitting':'Submitting...',
      }
    }
  }
</script>

<style scoped>

</style>
