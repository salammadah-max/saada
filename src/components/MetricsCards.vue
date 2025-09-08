<template>
  <v-row class="mb-6">
    <v-col
      v-for="metric in metrics"
      :key="metric.title"
      cols="12"
      sm="6"
      md="3"
    >
      <v-card
        :color="metric.color"
        class="pa-4 rounded-lg metric-card"
        elevation="0"
      >
        <div class="d-flex justify-space-between align-start mb-2">
          <div>
            <h3 class="text-h4 font-weight-bold text-white mb-1">
              {{ metric.value }}
            </h3>
            <p class="text-subtitle-2 text-white mb-2">
              {{ metric.title }}
            </p>
            <div class="d-flex align-center">
              <span class="me-2" style="font-size: 16px;">{{ metric.trendIcon }}</span>
              <span class="text-caption text-white">
                {{ metric.change }}
              </span>
            </div>
          </div>
          <div class="metric-icon">
            <span style="font-size: 32px; opacity: 0.8;">{{ metric.icon }}</span>
          </div>
        </div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script setup>
import { ref } from 'vue'

const metrics = ref([
  {
    title: 'إجمالي المشاريع',
    value: '24',
    change: 'زيادة 5 من الشهر الماضي',
    trendIcon: '📈',
    icon: '📊',
    color: 'success'
  },
  {
    title: 'المشاريع المكتملة',
    value: '10',
    change: 'زيادة 6 من الشهر الماضي',
    trendIcon: '📈',
    icon: '✅',
    color: 'grey-lighten-1'
  },
  {
    title: 'المشاريع الجارية',
    value: '12',
    change: 'زيادة 2 من الشهر الماضي',
    trendIcon: '📈',
    icon: '🔄',
    color: 'grey-lighten-1'
  },
  {
    title: 'المشاريع المعلقة',
    value: '2',
    change: 'قيد المناقشة',
    trendIcon: '⏳',
    icon: '⏸️',
    color: 'grey-lighten-1'
  }
])
</script>

<style scoped>
.metric-card {
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  cursor: pointer;
}

.metric-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.15);
}

.metric-icon {
  opacity: 0.7;
  transition: opacity 0.2s ease;
}

.metric-card:hover .metric-icon {
  opacity: 1;
}
</style>
