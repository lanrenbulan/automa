<template>
  <shared-card
    v-for="workflow in workflows"
    :key="workflow.id"
    :data="workflow"
    :show-details="false"
    @execute="RendererWorkflowService.executeWorkflow(workflow)"
    @click="$router.push(`/workflows/${$event.id}/shared`)"
  />
</template>
<script setup>
import { computed } from 'vue';
import { useSharedWorkflowStore } from '@/stores/sharedWorkflow';
import { arraySorter } from '@/utils/helper';
import SharedCard from '@/components/newtab/shared/SharedCard.vue';
import RendererWorkflowService from '@/service/renderer/RendererWorkflowService';

const props = defineProps({
  search: {
    type: String,
    default: '',
  },
  sort: {
    type: Object,
    default: () => ({
      by: '',
      order: '',
    }),
  },
});

const sharedWorkflowStore = useSharedWorkflowStore();

const workflows = computed(() => {
  const filtered = sharedWorkflowStore.toArray.filter(({ name }) =>
    name.toLocaleLowerCase().includes(props.search.toLocaleLowerCase())
  );

  return arraySorter({
    data: filtered,
    key: props.sort.by,
    order: props.sort.order,
  });
});
</script>
