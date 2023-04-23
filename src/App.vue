<template>
  <EnneagramAssessment />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import EnneagramAssessment from "./components/EnneagramAssessment.vue";

export default defineComponent({
  name: "App",
  components: {
    EnneagramAssessment,
  },
});
</script>

<style></style>
