<template>
  <div>
    <Layout class-prefix="layout">
      <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
      <Tabs :data-source="recordTypeList" :value.sync="record.type"/>
      <div class="createdAt">
        <FormItem field-name="日期"
                  type="date"
                  placeholder="在这里输入日期"
                  :value.sync="record.createdAt"/>
      </div>
      <div class="notes">
        <FormItem field-name="备注"
                  placeholder="在这里输入备注"
                  :value.sync="record.notes"/>
      </div>
      <Tags @update:value="record.tags = $event" ref="tags"/>
    </Layout>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import NumberPad from '@/components/Money/NumberPad.vue';
import FormItem from '@/components/Money/FormItem.vue';
import Tags from '@/components/Money/Tags.vue';
import {Component} from 'vue-property-decorator';
import recordTypeList from '@/constants/recordTypeList';
import Tabs from '@/components/Tabs.vue';

@Component({
  components: {Tabs, Tags, FormItem, NumberPad}
})
export default class Money extends Vue {
  get recordList(){
    return this.$store.state.recordList
  }

  recordTypeList = recordTypeList

  // eslint-disable-next-line no-undef
  record:RecordItem = {
    tags: [],
    notes: '',
    type: '-',
    amount: 0,
    createdAt: new Date().toISOString()
  };

  created(){
    this.$store.commit('fetchRecords')
  }

  saveRecord() {
    if (!this.record.tags || this.record.tags.length === 0){
      return window.alert('请至少选择一个标签')
    }
    this.$store.commit('createRecord', this.record)
    const tags = this.$refs.tags as any
    tags.selectedTags = []
    if(this.$store.state.createRecordError === null){
      window.alert('已保存')
      this.record.notes = ''
    }
  }

}
</script>

<style lang="scss" scoped>
::v-deep .layout-content {
  display: flex;
  flex-direction: column-reverse;
}
.notes {
  padding: 12px 0;
}

</style>

