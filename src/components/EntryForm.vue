<template>
  <DialogForm ref="dialogForm">
    <template v-slot:header>
      <h4 class="modal-title" v-if="insertMode">{{ label.title_new }}</h4>
      <h4 class="modal-title" v-if="updateMode">{{ label.title_edit }}</h4>
    </template>
    <template v-slot:default>
        <div class="row row-height">
          <div class="col-height col-md-4">
            <label for="account">{{ label.account_label }}</label>
            <InputMask ref="account" v-model="localData.account" id="account" name="account" picture="XXXXXXXXXXXX" :disabled="disabledKeyField"/> 
            <span v-if="v$.account.$error" class="has-error">{{ v$.account.$errors[0].$message }}</span>
          </div>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-4">
            <label for="amount">{{ label.amount_label }}</label>
            <InputMoney ref="amount" v-model="localData.amount" id="amount" name="amount" decimal="2" /> 
            <span v-if="v$.amount.$error" class="has-error">{{ v$.amount.$errors[0].$message }}</span>
          </div>
          <div class="col-height col-md-3">
            <label for="pincode">{{ label.pincode_label }}</label>
            <InputMask ref="pincode" v-model="localData.pincode" id="pincode" name="pincode" picture="XXXXXXXX" /> 
            <span v-if="v$.pincode.$error" class="has-error">{{ v$.pincode.$errors[0].$message }}</span>
          </div>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-4">
            <label for="effectdate">{{ label.effectdate_label }}:</label>
            <InputDate ref="effectdate" v-model="localData.effectdate" id="effectdate" name="effectdate" /> 
            <span v-if="v$.effectdate.$error" class="has-error">{{ v$.effectdate.$errors[0].$message }}</span>
          </div>
          <div class="col-height col-md-3">
            <label for="edittime">{{label.effecttime_label}}:</label>
            <InputTime ref="effecttime" v-model="localData.effecttime" id="effecttime" name="effecttime" /> 
            <span v-if="v$.effecttime.$error" class="has-error">{{ v$.effecttime.$errors[0].$message }}</span>
          </div>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-1">
            <label for="age">{{ label.age_label }}</label>
          </div>
          <div class="col-height col-md-2">
            <InputNumber ref="age" v-model="localData.age" id="age" name="age" /> 
            <span v-if="v$.age.$error" class="has-error">{{ v$.age.$errors[0].$message }}</span>
          </div>
          <div class="col-height col-md-5">
            <div class="form-check">
              <input ref="domestic" type="checkbox" id="domestic" :true-value="1" :false-value="0" v-model="localData.domestic" class="form-control input-md form-check-input"/>
              <label for="domestic" class="form-check-label">{{ label.domestic_label }}</label>
              <span v-if="v$.domestic.$error" class="has-error">{{ v$.domestic.$errors[0].$message }}</span>
            </div>
          </div>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-1">
            <label>{{ label.gender_label }}</label>
          </div>
          <div class="col-height col-md-2">
            <div class="form-check">
            <input ref="gender" type="radio" id="male" value="M" v-model="localData.gender" class="form-control input-md form-check-input"/>
            <label for="male" class="form-check-label">{{ label.male_label }}</label>
            </div>
          </div>
          <div class="col-height col-md-2">
            <div class="form-check">
            <input ref="gender" type="radio" id="female" value="F" v-model="localData.gender" class="form-control input-md form-check-input"/>
            <label for="female" class="form-check-label">{{ label.female_label }}</label>
            </div>
          </div>
          <span v-if="v$.gender.$error" class="has-error">{{ v$.gender.$errors[0].$message }}</span>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-1">
            <label>{{ label.licenses_label }}</label>
          </div>    
        <template v-for="item in dataCategory.licenses" :key="item.key">
          <div class="col-height col-md-2">
            <div class="form-check">
              <input ref="licenses" type="checkbox" :id="item.key" :value="item.key" v-model="localData.licenses" class="form-control input-md form-check-input"/>
              <label :for="item.key" class="form-check-label">{{item.text}}</label>
            </div>
          </div>
        </template>
        <span v-if="v$.licenses.$error" class="has-error">{{ v$.licenses.$errors[0].$message }}</span>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-3">
            <label>{{ label.marrystatus_label }}</label>
            <select ref="marrystatus" v-model="localData.marrystatus" class="form-control input-md">
              <option value=""></option>          
              <option v-for="item in dataCategory.marrystatus" :key="item.key" :value="item.key">{{item.text}}</option>
            </select>
            <span v-if="v$.marrystatus.$error" class="has-error">{{ v$.marrystatus.$errors[0].$message }}</span>
          </div>
          <div class="col-height col-md-3">
            <label>{{ label.languages_label }}</label>
            <select ref="languages" v-model="localData.languages" class="form-control input-md" multiple>
              <option v-for="item in localCategory.languages" :key="item.key" :value="item.key">{{item.text}}</option>
            </select>
            <span v-if="v$.languages.$error" class="has-error">{{ v$.languages.$errors[0].$message }}</span>
          </div>
        </div>
        <div class="row row-height">
          <div class="col-height col-md-8">
            <label for="remark">{{ label.remark_label }}</label>
            <input ref="remark" type="text" v-model="localData.remark" id="remark" class="form-control input-md" maxlength="100" />
          </div>
        </div>
    </template>
    <template v-slot:footer>
      <button class="btn btn-dark btn-sm" @click="saveClick" v-if="insertMode"><em class="fa fa-save fa-btn-icon"></em>{{ labels.save_button }}</button>
      <button class="btn btn-dark btn-sm" @click="updateClick" v-if="updateMode"><em class="fa fa-save fa-btn-icon"></em>{{ labels.update_button }}</button>
      <button class="btn btn-dark btn-sm" data-dismiss="modal"><em class="fa fa-close fa-btn-icon"></em>{{ labels.cancel_button }}</button>
    </template>
  </DialogForm>
</template>
<script>
import { ref, computed } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, helpers, between } from '@vuelidate/validators';
import $ from "jquery";
import { DEFAULT_CONTENT_TYPE, API_URL }  from '@/assets/apputil.js';
import { startWaiting, stopWaiting, submitFailure }  from '@/assets/apputil.js'
import { confirmUpdate, confirmSave, confirmDelete, successbox } from '@/assets/apputil.js'
import InputDate from '@/controls/InputDate.vue';
import InputTime from '@/controls/InputTime.vue';
import InputNumber from '@/controls/InputNumber.vue';
import InputMoney from '@/controls/InputMoney.vue';
import InputMask from '@/controls/InputMask.vue';
import DialogForm from './DialogForm.vue';

const defaultData = {
  account: '',
  amount: 0.00,
  pincode: "",
  effectdate: "",
  effecttime: "",
  age: 0,
  domestic: "1",
  gender: "M",
  licenses: [],
  marrystatus: "",
  languages: [],
  remark: "",
};

export default {
  components: {
    DialogForm, InputDate, InputTime, InputNumber, InputMoney, InputMask
  },
  props: {
    modes: Object,
    labels: Object,
    dataCategory: Object
  },
  emits: ["data-saved","data-updated","data-deleted"],
  setup(props) {
    const mode = ref({action: "new", ...props.modes});
    const label = ref({...props.labels});
    const localData = ref({ ...defaultData }); 
    const localCategory = ref({...props.dataCategory});
    const disabledKeyField = ref(false);
    const requiremsg = ref(props.labels.empty_alert);
    const requiredMessage = () => {
      return helpers.withMessage(requiremsg, required);
    }
    const validateRules = computed(() => { return {
      account: { required: requiredMessage() },
      amount: { required: requiredMessage() },
      pincode: { required: requiredMessage() },
      effectdate: { required: requiredMessage() },
      effecttime: { required: requiredMessage() },
      age: { required: requiredMessage(), between: between(1,150) },
      domestic: { required: requiredMessage() },
      gender: { required: requiredMessage() },
      marrystatus: { required: requiredMessage() },
      licenses: { required: requiredMessage() },
      languages: { required: requiredMessage() },
    } });
    const v$ = useVuelidate(validateRules, localData, { $lazy: true, $autoDirty: true });
    return { mode, label, v$, localData, localCategory, disabledKeyField, requiremsg };
  },
  computed: {
    insertMode() {
      return this.mode.action=="insert" || this.mode.action=="new";
    },
    updateMode() {
      return this.mode.action=="update" || this.mode.action=="edit";
    },
  },
  mounted() {
    this.$nextTick(function () {
      $("#modaldialog_layer").find(".modal-dialog").draggable();
    });
  },
  methods: {
    reset(newData,newMode) {
      if(newData) this.localData = {...newData};
      if(newMode) this.mode = {...newMode};
    },
    setLabel(newLabel) {
      if(newLabel) this.label = {...newLabel};
      this.requiremsg = newLabel.empty_alert;
    },
    submit() {      
      this.$emit('update:formData', this.localData);
    },
    async saveClick() {
      console.log("click: save");
      let valid = await this.validateForm();
      if(!valid) return;
      this.startSaveRecord();
    },
    async updateClick() {
      console.log("click: update");
      let valid = await this.validateForm();
      if(!valid) return;
      this.startUpdateRecord();
    },
    async validateForm(focusing=true) {
      const valid = await this.v$.$validate();
      console.log("validate form: valid",valid);
      console.log("error:",this.v$.$errors);
      if(!valid) {
        if(focusing) {
          this.focusFirstError();
        }
        return false;
      }
      return true;
    },
    focusFirstError() {
      if(this.v$.$errors && this.v$.$errors.length > 0) {
        let input = this.v$.$errors[0].$property;
        let el = this.$refs[input];
        if(el) el.focus();
      }
    },
    showDialog() {
      //$("#modaldialog_layer").modal("show");
      $(this.$refs.dialogForm.$el).modal("show");
    },  
    hideDialog() {
      //$("#modaldialog_layer").modal("hide");
      $(this.$refs.dialogForm.$el).modal("hide");
    },
    resetRecord() {
      //reset to default data 
      this.reset(defaultData,{action:"insert"});
      //reset validator
      this.v$.$reset();
      //enable key field
      this.disabledKeyField = false;
    },
    startInsertRecord() {
      console.log("dialogForm",this.$refs.dialogForm);
      this.resetRecord();
      this.showDialog();
    },
    startSaveRecord() {
      confirmSave(() => {
        this.saveRecord(this.localData);
      });
    },
    startUpdateRecord() {
      confirmUpdate(() => { 
        this.updateRecord(this.localData);
      });
    },
    startDeleteRecord(dataKeys) {
      confirmDelete(Object.values(dataKeys),() => {
        this.deleteRecord(dataKeys);
      });
    },
    saveRecord(dataRecord) {
        let jsondata = {ajax: true};
        Object.assign(jsondata,dataRecord);
        startWaiting();
        $.ajax({
          url: API_URL+"/api/demo002/insert",
          data: jsondata,
          type: "POST",
          dataType: "html",
          contentType: DEFAULT_CONTENT_TYPE,
          error : function(transport,status,errorThrown) {
            console.error("error: status",status,"errorThrown",errorThrown);
            submitFailure(transport,status,errorThrown);
          },
          success: (data) => {
            stopWaiting();
            console.log("success",data);
            successbox(() => {
              //reset data for new record insert
              this.resetRecord();
              this.$refs.account.focus();
            });
            this.$emit('data-saved',dataRecord,data);
          }
      });
    },
    updateRecord(dataRecord) {
        let jsondata = {ajax: true};
        Object.assign(jsondata,dataRecord);
        startWaiting();
        $.ajax({
          url: API_URL+"/api/demo002/update",
          data: jsondata,
          type: "POST",
          dataType: "html",
          contentType: DEFAULT_CONTENT_TYPE,
          error : function(transport,status,errorThrown) {
            console.error("error: status",status,"errorThrown",errorThrown);
            submitFailure(transport,status,errorThrown);
          },
          success: (data) => {
            stopWaiting();
            console.log("success",data);
            successbox(() => {
              this.hideDialog();
            });
            this.$emit('data-updated',dataRecord,data);
          }
      });
    },
    retrieveRecord(dataKeys) {
      let jsondata = {ajax: true};
      Object.assign(jsondata,dataKeys);
      startWaiting();
      $.ajax({
        url: API_URL+"/api/demo002/retrieve",
        data: jsondata,
        type: "POST",
        dataType: "json",
        contentType: DEFAULT_CONTENT_TYPE,
        error : function(transport,status,errorThrown) {
          console.error("retrieveRecord: error: status",status,"errorThrown",errorThrown);
          submitFailure(transport,status,errorThrown);
        },
        success: (data) => {
          stopWaiting();
          console.log("retrieveRecord: success",data);
          if(data.body.dataset) {
            this.reset(data.body.dataset,{action:"edit"});
            this.v$.$reset();
            this.disabledKeyField = true;
            this.showDialog();
          }
        }
      });	
    },
    deleteRecord(dataKeys) {
      let jsondata = {ajax: true};
      Object.assign(jsondata,dataKeys);
      startWaiting();
      $.ajax({
        url: API_URL+"/api/demo002/remove",
        data: jsondata,
        type: "POST",
        dataType: "json",
        contentType: DEFAULT_CONTENT_TYPE,
        error : function(transport,status,errorThrown) {
          console.error("deleteRecord: error: status",status,"errorThrown",errorThrown);
          submitFailure(transport,status,errorThrown);
        },
        success: (data) => {
          stopWaiting();
          console.log("deleteRecord: success",data);
          successbox();
          this.$emit('data-deleted',dataKeys,data);
        }
      });	
    },
  }
};
</script>
