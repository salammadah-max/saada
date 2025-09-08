<template>
  <v-row class="mb-6">
    <!-- Project Analytics -->
    <v-col cols="12" md="4">
      <v-card class="pa-4 rounded-lg" elevation="1">
        <div class="d-flex justify-space-between align-center mb-4">
          <h3 class="text-h6 font-weight-bold">تحليلات المشاريع</h3>
          <v-btn
            icon
            variant="text"
            size="small"
          >
            <span style="font-size: 20px;">⚙️</span>
          </v-btn>
        </div>
        
        <!-- Chart -->
        <div class="chart-container">
          <div class="d-flex justify-space-between align-end mb-2">
            <div
              v-for="(day, index) in chartData"
              :key="index"
              class="chart-bar-container"
            >
              <div
                class="chart-bar"
                :class="{ 'active': day.active }"
                :style="{ height: day.height + '%' }"
              ></div>
              <div class="chart-label">{{ day.label }}</div>
              <div v-if="day.percentage" class="chart-percentage">
                {{ day.percentage }}
              </div>
            </div>
          </div>
        </div>
      </v-card>
    </v-col>

    <!-- Reminders -->
    <v-col cols="12" md="4">
      <v-card class="pa-4 rounded-lg" elevation="1">
        <div class="d-flex justify-space-between align-center mb-4">
          <h3 class="text-h6 font-weight-bold">التذكيرات</h3>
          <v-btn
            icon
            variant="text"
            size="small"
          >
            <span style="font-size: 20px;">➕</span>
          </v-btn>
        </div>
        
        <div class="reminder-item">
          <div class="d-flex align-center mb-3">
            <span class="me-3" style="font-size: 24px;">📅</span>
            <div>
              <h4 class="text-subtitle-1 font-weight-bold">اجتماع مع شركة آرك</h4>
              <p class="text-caption text-medium-emphasis">
                02:00 م - 04:00 م
              </p>
            </div>
          </div>
          <v-btn
            color="success"
            variant="flat"
            size="small"
            class="w-100"
          >
            <span class="me-2">📹</span>
            بدء الاجتماع
          </v-btn>
        </div>
      </v-card>
    </v-col>

    <!-- Project List -->
    <v-col cols="12" md="4">
      <v-card class="pa-4 rounded-lg" elevation="1">
        <div class="d-flex justify-space-between align-center mb-4">
          <h3 class="text-h6 font-weight-bold">المشاريع</h3>
          <v-btn
            color="primary"
            variant="flat"
            size="small"
          >
            <span class="me-1">➕</span>
            جديد
          </v-btn>
        </div>
        
        <div class="project-list">
          <div
            v-for="project in projects"
            :key="project.id"
            class="project-item d-flex align-center mb-3"
          >
            <div class="project-icon me-3">
              <span style="font-size: 20px;">{{ project.icon }}</span>
            </div>
            <div class="flex-grow-1">
              <h4 class="text-subtitle-2 font-weight-bold mb-1">
                {{ project.title }}
              </h4>
              <p class="text-caption text-medium-emphasis">
                {{ project.dueDate }}
              </p>
            </div>
          </div>
        </div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script setup>
import { ref } from 'vue'

const chartData = ref([
  { label: 'أ', height: 30, active: false },
  { label: 'إ', height: 60, active: true },
  { label: 'ث', height: 74, active: true, percentage: '74%' },
  { label: 'أ', height: 45, active: false },
  { label: 'خ', height: 25, active: false },
  { label: 'ج', height: 40, active: false },
  { label: 'س', height: 35, active: false }
])

const projects = ref([
  {
    id: 1,
    title: 'تطوير واجهات برمجة التطبيقات',
    dueDate: 'تاريخ الاستحقاق: 26 نوفمبر 2024',
    icon: '🔗'
  },
  {
    id: 2,
    title: 'تصميم واجهة المستخدم',
    dueDate: 'تاريخ الاستحقاق: 28 نوفمبر 2024',
    icon: '🎨'
  },
  {
    id: 3,
    title: 'اختبار النظام',
    dueDate: 'تاريخ الاستحقاق: 30 نوفمبر 2024',
    icon: '🧪'
  },
  {
    id: 4,
    title: 'نشر التطبيق',
    dueDate: 'تاريخ الاستحقاق: 2 ديسمبر 2024',
    icon: '🚀'
  },
  {
    id: 5,
    title: 'التوثيق التقني',
    dueDate: 'تاريخ الاستحقاق: 5 ديسمبر 2024',
    icon: '📚'
  }
])
</script>

<style scoped>
.chart-container {
  height: 200px;
  display: flex;
  align-items: end;
}

.chart-bar-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 2px;
}

.chart-bar {
  width: 20px;
  background: #e0e0e0;
  border-radius: 4px 4px 0 0;
  transition: all 0.3s ease;
  margin-bottom: 8px;
}

.chart-bar.active {
  background: linear-gradient(to top, #4caf50, #8bc34a);
}

.chart-label {
  font-size: 12px;
  font-weight: 500;
  color: #666;
}

.chart-percentage {
  font-size: 10px;
  font-weight: bold;
  color: #4caf50;
  margin-top: 2px;
}

.reminder-item {
  background: #f8f9fa;
  padding: 16px;
  border-radius: 8px;
  border-left: 4px solid #4caf50;
}

.project-item {
  padding: 12px;
  border-radius: 8px;
  transition: background 0.2s ease;
}

.project-item:hover {
  background: #f5f5f5;
}

.project-icon {
  width: 40px;
  height: 40px;
  background: #f0f0f0;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
