<template>
  <div>
    <div style="margin: 20px">
      <Button @click="changeType('base')" style="margin: 20px">基础组件</Button>
      <Button @click="changeType('vModel')" style="margin: 20px">V-Model</Button>
    </div>
    <BaseEditor v-if="editorType === 'base'" />
    <VModel v-if="editorType === 'vModel'" />
  </div>
</template>
<script>
import Vue from 'vue';
import VModel from './components/VModel.vue';
import BaseEditor from './components/BaseC.vue';

export default Vue.extend({
  components: { VModel, BaseEditor },
  data() {
    return {
      editorType: 'base',
    };
  },

  methods: {
    changeType(type) {
      this.editorType = type;
    },
  },

  beforeDestroy() {},
});
</script>
