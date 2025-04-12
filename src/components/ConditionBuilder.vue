<template>
    <!-- 条件列表 -->
    <div class="conditions-container">
        <div style="width: 100%; display: flex;align-items: center;">
            <VerticalDivider v-if="conditions.length > 1">
                <a-radio-group 
                    v-model:value="rootRelation" 
                    size="small"
                    button-style="solid"
                    class="vertical-radio-group"
                >
                    <a-radio-button value="AND">并且</a-radio-button>
                    <a-radio-button value="OR">或者</a-radio-button>
                </a-radio-group>
                </VerticalDivider>
            <a-space direction="vertical">
                <div v-for="(item, index) in conditions" :key="item.id" class="condition-item">
            <!-- 条件连接线 -->

            <!-- 单个条件 -->
            <a-card v-if="!item.isGroup" size="small">
                <a-space>
                <a-select v-model:value="item.field" style="width: 120px;">
                    <a-select-option value="age">年龄</a-select-option>
                    <a-select-option value="name">姓名</a-select-option>
                </a-select>
                <a-select v-model:value="item.operator" style="width: 80px;">
                    <a-select-option value="=">=</a-select-option>
                    <a-select-option value=">">></a-select-option>
                </a-select>
                <a-input v-model:value="item.value" style="width: 120px;" />
                <a-button v-if="conditions.length > 1" @click="convertToGroup(index)">并且满足</a-button>
                <a-button @click="removeCondition(index)" danger>删除</a-button>
                </a-space>
            </a-card>

            <!-- 条件组 -->
            <a-card v-else size="small" :title="`条件组 (${item.groupRelation === 'AND' ? '并且' : '或者'})`">
                <a-space style="width: 100%">
                    <VerticalDivider>
                    <a-radio-group 
                        v-model:value="item.groupRelation" 
                        size="small"
                        button-style="solid"
                        class="vertical-radio-group"
                    >
                        <a-radio-button value="AND">并且</a-radio-button>
                        <a-radio-button value="OR">或者</a-radio-button>
                    </a-radio-group>
                    </VerticalDivider>
                    <a-space direction="vertical">
                    <div v-for="(cond, condIdx) in item.conditions" :key="cond.id" class="group-condition">
                    <a-space>
                    <a-select v-model:value="cond.field" style="width: 120px;">
                        <a-select-option value="age">年龄</a-select-option>
                        <a-select-option value="name">姓名</a-select-option>
                    </a-select>
                    <a-select v-model:value="cond.operator" style="width: 80px;">
                        <a-select-option value="=">=</a-select-option>
                        <a-select-option value=">">></a-select-option>
                    </a-select>
                    <a-input v-model:value="cond.value" style="width: 120px;" />
                    <a-button @click="removeGroupCondition(index, condIdx)" danger>删除</a-button>
                    <a-button 
                        v-if="condIdx === item.conditions.length - 1" 
                        @click="addGroupCondition(index)"
                    >
                        并且满足
                    </a-button>
                    </a-space>
                </div>
                    </a-space>
                
                </a-space>
            </a-card>
            </div>
            </a-space>
        
        </div>
        <a-space>
          <a-button @click="addCondition" type="primary" size="small">添加条件</a-button>
        </a-space>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  import VerticalDivider from './VerticalDivider.vue'

  interface Condition {
    id: string;
    field: string;
    operator: string;
    value: string;
  }
  
  interface ConditionGroup {
    id: string;
    isGroup: true;
    groupRelation: 'AND' | 'OR';
    conditions: Condition[];
  }
  
  type ConditionItem = Condition | ConditionGroup;
  
  const rootRelation = ref<'AND' | 'OR'>('AND');
  const conditions = ref<ConditionItem[]>([]);
  
  const addCondition = () => {
    conditions.value.push({
      id: Date.now().toString(),
      field: 'age',
      operator: '=',
      value: ''
    });
  };
  
  const convertToGroup = (index: number) => {
    const condition = conditions.value[index] as Condition;
    conditions.value[index] = {
      id: condition.id,
      isGroup: true,
      groupRelation: 'AND',
      conditions: [
        { ...condition },
        {
          id: Date.now().toString(),
          field: 'age',
          operator: '=',
          value: ''
        }
      ]
    };
  };
  
  const toggleRootRelation = () => {
    rootRelation.value = rootRelation.value === 'AND' ? 'OR' : 'AND';
  };
  
  const toggleGroupRelation = (groupIndex: number) => {
    const group = conditions.value[groupIndex] as ConditionGroup;
    group.groupRelation = group.groupRelation === 'AND' ? 'OR' : 'AND';
  };
  
  const addGroupCondition = (groupIndex: number) => {
    const group = conditions.value[groupIndex] as ConditionGroup;
    group.conditions.push({
      id: Date.now().toString(),
      field: 'age',
      operator: '=',
      value: ''
    });
  };
  
  const removeCondition = (index: number) => {
    conditions.value.splice(index, 1);
  };
  
  const removeGroupCondition = (groupIndex: number, condIndex: number) => {
    const group = conditions.value[groupIndex] as ConditionGroup;
    group.conditions.splice(condIndex, 1);
    
    if (group.conditions.length === 1) {
      conditions.value[groupIndex] = {
        ...group.conditions[0],
        id: group.id
      };
    }
  };
  </script>
  
  <style scoped>
  .condition-builder {
    max-width: 800px;
    margin: 0 auto;
  }
  
  .conditions-container {
    margin-bottom: 16px;
  }
  
  .condition-item {
    display: flex;
    flex-direction: column;
  }
  
  .group-condition {
    display: flex;
    flex-direction: column;
  }
  
  pre {
    background: #f5f5f5;
    padding: 12px;
    border-radius: 4px;
    overflow: auto;
  }
  
  /* 垂直排列的Radio组样式 */
.vertical-radio-group {
  display: flex;
  flex-direction: column;
}

.vertical-radio-group :deep(.ant-radio-button-wrapper) {
  display: block;
  text-align: center;
  line-height: 1.5;
  padding: 0 4px;
}
  </style>