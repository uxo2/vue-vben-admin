<template>
  <div style="width: 100%">
    <div v-if="$slots.headerTop" style="margin: 5px">
      <slot name="headerTop"></slot>
    </div>
    <div class="flex items-center">
      <slot name="tableTitle" v-if="$slots.tableTitle"></slot>
      <TableTitle
        v-if="!$slots.tableTitle && title"
        :helpMessage="titleHelpMessage"
        :title="title"
      />
      <div :class="`${prefixCls}__toolbar`">
        <slot name="toolbar"></slot>
        <Divider type="vertical" v-if="$slots.toolbar && showTableSetting" />
        <TableSetting
          :setting="tableSetting"
          v-if="showTableSetting"
          @columnsChange="handleColumnChange"
        />
      </div>
    </div>
  </div>
</template>
<script lang="ts">
  import type { TableSetting, ColumnChangeParam } from '../types/table'
  import type { PropType } from 'vue'
  import { defineComponent } from 'vue'
  import { Divider } from 'ant-design-vue'
  import TableSettingComponent from './settings/index.vue'
  import TableTitle from './TableTitle.vue'
  import { useDesign } from '/@/hooks/web/useDesign'

  export default defineComponent({
    name: 'BasicTableHeader',
    components: {
      Divider,
      TableTitle,
      TableSetting: TableSettingComponent
    },
    props: {
      title: {
        type: [Function, String] as PropType<string | ((data: Recordable) => string)>
      },
      tableSetting: {
        type: Object as PropType<TableSetting>
      },
      showTableSetting: {
        type: Boolean
      },
      titleHelpMessage: {
        type: [String, Array] as PropType<string | string[]>,
        default: ''
      }
    },
    emits: ['columns-change'],
    setup(_, { emit }) {
      const { prefixCls } = useDesign('basic-table-header')
      function handleColumnChange(data: ColumnChangeParam[]) {
        emit('columns-change', data)
      }
      return { prefixCls, handleColumnChange }
    }
  })
</script>
<style lang="less">
  @prefix-cls: ~'@{namespace}-basic-table-header';

  .@{prefix-cls} {
    &__toolbar {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: flex-end;

      > * {
        margin-right: 8px;
      }
    }
  }
</style>
