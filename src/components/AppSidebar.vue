<template>
  <v-navigation-drawer
    v-model="drawer"
    :rail="rail"
    permanent
    :width="280"
    class="sidebar"
  >
    <!-- Logo Section -->
    <div class="pa-4 text-center">
      <div class="d-flex align-center justify-center mb-4">
        <div class="logo-icon me-3">💰</div>
        <h2 class="text-h5 font-weight-bold text-primary">نظام المالية</h2>
      </div>
    </div>

    <!-- Menu Section -->
    <v-list class="px-3">
      <v-list-subheader class="text-uppercase font-weight-bold text-caption">
        القائمة الرئيسية
      </v-list-subheader>
      
      <v-list-item
        v-for="item in menuItems"
        :key="item.title"
        :to="item.to"
        :active="item.active"
        class="mb-1 rounded-lg"
        :class="{ 'active-item': item.active }"
      >
        <template v-slot:prepend>
          <span class="me-3" :style="`font-size: 20px; color: ${item.active ? '#4caf50' : '#666'}`">
            {{ item.icon }}
          </span>
        </template>
        
        <v-list-item-title class="font-weight-medium">
          {{ item.title }}
        </v-list-item-title>
        
        <template v-slot:append v-if="item.badge">
          <v-chip
            :color="item.badgeColor"
            size="small"
            class="text-white"
          >
            {{ item.badge }}
          </v-chip>
        </template>
      </v-list-item>
    </v-list>

    <!-- General Section -->
    <v-list class="px-3 mt-4">
      <v-list-subheader class="text-uppercase font-weight-bold text-caption">
        عام
      </v-list-subheader>
      
      <v-list-item
        v-for="item in generalItems"
        :key="item.title"
        :to="item.to"
        class="mb-1 rounded-lg"
      >
        <template v-slot:prepend>
          <span class="me-3" style="font-size: 20px; color: #666;">
            {{ item.icon }}
          </span>
        </template>
        
        <v-list-item-title class="font-weight-medium">
          {{ item.title }}
        </v-list-item-title>
      </v-list-item>
    </v-list>

    <!-- Mobile App Card -->
    <div class="pa-3 mt-auto">
      <v-card
        color="primary"
        class="pa-4 rounded-lg"
        elevation="0"
      >
        <div class="d-flex align-center mb-2">
          <span class="me-3" style="font-size: 24px;">📱</span>
          <div>
            <h4 class="text-h6 font-weight-bold text-white mb-1">
              تطبيق الجوال
            </h4>
            <p class="text-caption text-white">
              احصل على سهولة الوصول بطريقة أخرى
            </p>
          </div>
        </div>
        <v-btn
          color="white"
          variant="flat"
          size="small"
          class="text-primary font-weight-bold"
        >
          تحميل
        </v-btn>
      </v-card>
    </div>
  </v-navigation-drawer>
</template>

<script setup>
import { ref } from 'vue'

const drawer = ref(true)
const rail = ref(false)

const menuItems = ref([
  {
    title: 'لوحة التحكم',
    icon: '📊',
    to: '/',
    active: true,
    badge: null
  },
  {
    title: 'المهام',
    icon: '📋',
    to: '/tasks',
    active: false,
    badge: '12+',
    badgeColor: 'success'
  },
  {
    title: 'التقويم',
    icon: '📅',
    to: '/calendar',
    active: false,
    badge: null
  },
  {
    title: 'التحليلات',
    icon: '📈',
    to: '/analytics',
    active: false,
    badge: null
  },
  {
    title: 'الفريق',
    icon: '👥',
    to: '/team',
    active: false,
    badge: null
  }
])

const generalItems = ref([
  {
    title: 'الإعدادات',
    icon: '⚙️',
    to: '/settings'
  },
  {
    title: 'المساعدة',
    icon: '❓',
    to: '/help'
  },
  {
    title: 'تسجيل الخروج',
    icon: '🚪',
    to: '/logout'
  }
])
</script>

<style scoped>
.sidebar {
  background: #fafafa !important;
  border-right: 1px solid #e0e0e0;
}

.logo-icon {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #4caf50, #8bc34a);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: white;
}

.active-item {
  background: #4caf50 !important;
  color: white !important;
}

.active-item .v-list-item-title {
  color: white !important;
}

.v-list-item {
  transition: all 0.3s ease;
}

.v-list-item:hover {
  background: #f5f5f5;
  transform: translateX(4px);
}

.active-item:hover {
  background: #45a049 !important;
}
</style>
