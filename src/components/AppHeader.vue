<template>
  <v-app-bar
    :elevation="1"
    color="white"
    height="70"
    class="header-bar"
  >
    <!-- Logo -->
    <div class="d-flex align-center me-6">
      <div class="logo-icon me-3">💰</div>
      <h1 class="text-h6 font-weight-bold text-primary">نظام المالية</h1>
    </div>

    <!-- Search Bar -->
    <v-text-field
      v-model="searchQuery"
      placeholder="البحث في المهام..."
      prepend-inner-icon="🔍"
      variant="outlined"
      density="compact"
      hide-details
      class="search-field"
      style="max-width: 400px;"
    >
      <template v-slot:append>
        <span class="text-caption text-medium-emphasis">⌘F</span>
      </template>
    </v-text-field>

    <!-- Spacer -->
    <v-spacer></v-spacer>

    <!-- User Actions -->
    <div class="d-flex align-center ga-4">
      <!-- Messages -->
      <v-btn
        icon
        variant="text"
        color="grey-darken-1"
        size="small"
      >
        <span style="font-size: 20px;">✉️</span>
        <v-badge
          color="error"
          content="3"
          offset-x="8"
          offset-y="8"
        >
        </v-badge>
      </v-btn>

      <!-- Notifications -->
      <v-btn
        icon
        variant="text"
        color="grey-darken-1"
        size="small"
      >
        <span style="font-size: 20px;">🔔</span>
        <v-badge
          color="error"
          content="5"
          offset-x="8"
          offset-y="8"
        >
        </v-badge>
      </v-btn>

      <!-- User Profile -->
      <div class="d-flex align-center ga-2">
        <v-avatar
          color="pink"
          size="40"
        >
          <span class="text-white font-weight-bold">👤</span>
        </v-avatar>
        <div class="text-left">
          <div class="text-subtitle-2 font-weight-bold">أحمد محمد</div>
          <div class="text-caption text-medium-emphasis">ahmed@company.com</div>
        </div>
      </div>
    </div>
  </v-app-bar>
</template>

<script setup>
import { ref } from 'vue'

const searchQuery = ref('')
</script>

<style scoped>
.header-bar {
  border-bottom: 1px solid #e0e0e0;
  background: white !important;
}

.logo-icon {
  width: 32px;
  height: 32px;
  background: linear-gradient(135deg, #4caf50, #8bc34a);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  color: white;
}

.search-field {
  background: #f5f5f5;
  border-radius: 12px;
}

.search-field :deep(.v-field__outline) {
  border: none;
}

.search-field :deep(.v-field__input) {
  padding: 8px 12px;
}
</style>
