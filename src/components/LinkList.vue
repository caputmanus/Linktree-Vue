<template>
  <section class="list">
    <LinkCard
      v-for="(link, i) in links"
      :key="link.id ?? i"
      :link="link"
      :delay="i * 80"
    />
  </section>
</template>

<script setup>
import LinkCard from './LinkCard.vue'

defineProps({
  links: { type: Array, required: true },
})
</script>

<style scoped>
.list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: 100%;
}
</style>
