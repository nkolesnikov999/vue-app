<script setup>
  import Heart from "@/icons/Heart.vue";
  const { score = 0} = defineProps({
  score: Number,
});
</script>

<template>
  <div class="score">
    <div class="score_label">{{ score }}</div>
    <Heart />
  </div>
</template>

<style scoped>
.score {
  display: flex;
  align-items: center;
  gap: 10px;
  border-radius: 36px;
  padding: 12px 24px;
  width: 110px;
  height: 48px;
  box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.15);
  background: #cce8ff;
}

.score_label {
  font-family: var(--font-family);
  font-weight: 700;
  font-size: 16px;
  line-height: 150%;
  text-align: center;
  color: #008bfe;
}
</style>  