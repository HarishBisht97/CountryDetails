<template>
  <div class="countries-container">
    <Countries />
  </div>
</template>

<script>
import Countries from "@/components/Countries.vue";

export default {
  components: {
    Countries,
  },
};
</script>

<style>
.countries-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
}
</style>
