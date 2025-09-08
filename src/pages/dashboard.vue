<template>
  <v-app>
    <!-- Sidebar -->
    <AppSidebar />
    
    <!-- Main Content -->
    <v-main>
      <!-- Header -->
      <AppHeader />
      
      <!-- Dashboard Content -->
      <v-container class="pa-6" fluid>
        <!-- Dashboard Title & Actions -->
        <div class="d-flex justify-space-between align-center mb-6">
          <div>
            <h1 class="text-h4 font-weight-bold mb-2">لوحة التحكم</h1>
            <p class="text-subtitle-1 text-medium-emphasis">
              خطط، أول، وأنجز مهامك بسهولة
            </p>
          </div>
          <div class="d-flex ga-3">
            <v-btn
              color="success"
              variant="flat"
              size="large"
              class="px-6"
            >
              <span class="me-2">➕</span>
              إضافة مشروع
            </v-btn>
            <v-btn
              color="success"
              variant="outlined"
              size="large"
              class="px-6"
            >
              <span class="me-2">📥</span>
              استيراد البيانات
            </v-btn>
          </div>
        </div>

        <!-- Metrics Cards -->
        <MetricsCards />

        <!-- Analytics Section -->
        <AnalyticsSection />

        <!-- Team & Progress Section -->
        <TeamProgressSection />

        <!-- Footer -->
        <div class="text-center mt-8 pa-4">
          <p class="text-body-2 text-medium-emphasis">
            © 2024 ارتكاز للحلول البرمجية - erticaz.com
          </p>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import AppSidebar from '@/components/AppSidebar.vue'
import AppHeader from '@/components/AppHeader.vue'
import MetricsCards from '@/components/MetricsCards.vue'
import AnalyticsSection from '@/components/AnalyticsSection.vue'
import TeamProgressSection from '@/components/TeamProgressSection.vue'
</script>

<style scoped>
/* Global styles for the dashboard */
.v-main {
  background: #fafafa;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: #a8a8a8;
}
</style>
