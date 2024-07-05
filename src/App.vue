<!-- App.vue -->
<template>
  <div id="fswaitlayer"><img id="waitImage" class="waitimgclass" src="img/waiting.gif" width="50px" height="50px" alt="" /></div>
  <div class="pt-page pt-page-current pt-page-controller search-pager">
    <PageHeader ref="pageHeader" :labels="label" pid="demo002">
      <li><a href="javascript:void(0)" @click="changeLanguage('EN')" class="pagemenu-linker"><img src="img/lang/EN.png" alt="EN" class="img-lang" />&nbsp;{{ label.english_lang }}</a></li>
      <li><a href="javascript:void(0)" @click="changeLanguage('TH')" class="pagemenu-linker"><img src="img/lang/TH.png" alt="TH" class="img-lang" />&nbsp;{{ label.thai_lang }}</a></li>
      <hr class="menu-separator"/>
    </PageHeader>
    <SearchForm ref="searchForm" :labels="label" :dataCategory="dataCategory" @data-select="dataSelected" @data-insert="dataInsert"/>
  </div>
  <teleport to="#modaldialog">
    <EntryForm ref="entryForm" :labels="label" :dataCategory="dataCategory" @data-saved="dataSaved" @data-updated="dataUpdated" @data-deleted="dataDeleted" />
  </teleport>
</template>
<script>
import { ref } from 'vue';
import PageHeader from '@/controls/PageHeader.vue';
import SearchForm from '@/components/SearchForm.vue';
import EntryForm from '@/components/EntryForm.vue';
import { getLabelModel } from "@/assets/labelutil.js";
import { startApplication } from "@/assets/apputil.js";

export default {
  components: {
    PageHeader, SearchForm, EntryForm
  },
  setup() {
    const dataCategory = {
      marrystatus: [{key: "S", text: "Single"}, {key: "M", text: "Married"},{key: "D", text: "Divorce"},{key: "W", text: "Widow"}],
      licenses: [{key: "CAR", text: "Car"},{key: "TRUCK", text: "Truck"},{key: "BOAT", text: "Boat"}],
      languages: [{key: "TH", text: "Thai"},{key: "EN", text: "English"},{key: "CN", text: "Chinese"},{key: "KR", text: "Korea"},{key: "JP", text: "Japan"}]
    };
    let label = ref(getLabelModel());
    return { label, dataCategory };
  },
  mounted() {
    console.log("App: mounted ...");
    this.$nextTick(function () {
      //ensure ui completed then invoke startApplication 
      startApplication();
    });
  },
  methods: {
    changeLanguage(lang) {
      let labelModel = getLabelModel(lang);
      this.label = labelModel;
      this.$refs.pageHeader.setLabel(labelModel);
      this.$refs.searchForm.setLabel(labelModel);
      this.$refs.entryForm.setLabel(labelModel);
    },
    dataSelected(item,action) {
      //listen action from search form
      console.log("App: dataSelected",item,"action",action);
      if("edit"==action) {
        this.$refs.entryForm.retrieveRecord({account: item.account});
      } else if("delete"==action) {
        this.$refs.entryForm.startDeleteRecord({account: item.account});
      }
    },
    dataInsert(filters) {
      //listen action from search form
      console.log("App: record insert",filters);
      this.$refs.entryForm.startInsertRecord();
    },
    dataSaved(data,response) {
      //listen action from entry form when after saved
      console.log("App: record saved");
      console.log("data",data,"response",response);
      this.$refs.searchForm.search();
    },
    dataUpdated(data,response) {
      //listen action from entry form when after updated
      console.log("App: record updated");
      console.log("data",data,"response",response);
      this.$refs.searchForm.search();
    },
    dataDeleted(data,response) {
      //listen action from entry form when after deleted
      console.log("App: record deleted");
      console.log("data",data,"response",response);
      this.$refs.searchForm.search(true);
    },
  }
};
</script>