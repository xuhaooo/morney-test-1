<template>
  <ul class="tabs" :class="{[classPrefix + '-tabs-item']: classPrefix}">
    <li v-for="item of dataSource" :key="item.value"
      @click="select(item)"
      class="tabs-item"
      :class="liClass(item)"
    >{{item.text}}</li>
  </ul>
</template>

<script lang="ts">
import Vue from 'vue';
import {Component, Prop} from 'vue-property-decorator';

type dataSourceItem = {
  text: string
  value: string
}

@Component
export default class Tabs extends Vue {
  @Prop({required: true, type: Array}) dataSource!: dataSourceItem[]
  @Prop(String) readonly value!: string;
  @Prop(String) classPrefix?: string;

  liClass(item: dataSourceItem){
    return {[this.classPrefix+'-tabs-item']: this.classPrefix, selected: item.value === this.value}
  }

  select(item: dataSourceItem){
    this.$emit('update:value', item.value)
  }
}
</script>

<style lang="scss" scoped>
.tabs {
  background: #c4c4c4;
  display: flex;
  text-align: center;
  font-size: 24px;
  &-item {
    width: 50%;
    height: 64px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    &.selected::after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 4px;
      background-color: #333;
    }
  }
}
</style>