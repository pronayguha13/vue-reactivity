<template>
  <span v-if="shouldFetchProps">The data should be fetched...</span>
  <span v-else>The data should not be fetched...</span>
</template>
<script lang="ts">
import { defineComponent, ref, toRef } from 'vue';

export default defineComponent({
  name: 'WatchEffectDemo',
  props: {
    shouldFetch: {
      type: Boolean,
    },
  },
  setup(props: any) {
    // const { shouldFetch } = toRef(props);
    const shouldFetchProps = ref<boolean>(props.shouldFetch);

    return {
      shouldFetchProps,
    };
  },
});
</script>
