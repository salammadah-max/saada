<template>
  <v-row>
    <!-- Team Collaboration -->
    <v-col cols="12" md="4">
      <v-card class="pa-4 rounded-lg" elevation="1">
        <div class="d-flex justify-space-between align-center mb-4">
          <h3 class="text-h6 font-weight-bold">تعاون الفريق</h3>
          <v-btn
            color="primary"
            variant="flat"
            size="small"
          >
            <span class="me-1">➕</span>
            إضافة عضو
          </v-btn>
        </div>
        
        <div class="team-list">
          <div
            v-for="member in teamMembers"
            :key="member.id"
            class="team-member d-flex align-center mb-4"
          >
            <v-avatar
              :color="member.avatarColor"
              size="40"
              class="me-3"
            >
              <span class="text-white font-weight-bold">{{ member.initials }}</span>
            </v-avatar>
            
            <div class="flex-grow-1">
              <h4 class="text-subtitle-2 font-weight-bold mb-1">
                {{ member.name }}
              </h4>
              <p class="text-caption text-medium-emphasis mb-2">
                {{ member.task }}
              </p>
              <v-chip
                :color="member.statusColor"
                size="x-small"
                variant="flat"
              >
                {{ member.status }}
              </v-chip>
            </div>
          </div>
        </div>
      </v-card>
    </v-col>

    <!-- Project Progress -->
    <v-col cols="12" md="4">
      <v-card class="pa-4 rounded-lg" elevation="1">
        <div class="d-flex justify-space-between align-center mb-4">
          <h3 class="text-h6 font-weight-bold">تقدم المشاريع</h3>
          <v-btn
            icon
            variant="text"
            size="small"
          >
            <span style="font-size: 20px;">📊</span>
          </v-btn>
        </div>
        
        <!-- Donut Chart -->
        <div class="donut-chart-container">
          <div class="donut-chart">
            <div class="donut-center">
              <span class="text-h4 font-weight-bold">41%</span>
              <p class="text-caption text-medium-emphasis">مكتمل</p>
            </div>
          </div>
          
          <!-- Legend -->
          <div class="chart-legend mt-4">
            <div class="legend-item d-flex align-center mb-2">
              <div class="legend-dot completed me-2"></div>
              <span class="text-caption">مكتمل</span>
            </div>
            <div class="legend-item d-flex align-center mb-2">
              <div class="legend-dot in-progress me-2"></div>
              <span class="text-caption">قيد التنفيذ</span>
            </div>
            <div class="legend-item d-flex align-center">
              <div class="legend-dot pending me-2"></div>
              <span class="text-caption">معلق</span>
            </div>
          </div>
        </div>
      </v-card>
    </v-col>

    <!-- Time Tracker -->
    <v-col cols="12" md="4">
      <v-card class="pa-4 rounded-lg" elevation="1">
        <div class="d-flex justify-space-between align-center mb-4">
          <h3 class="text-h6 font-weight-bold">متتبع الوقت</h3>
          <v-btn
            icon
            variant="text"
            size="small"
          >
            <span style="font-size: 20px;">⏱️</span>
          </v-btn>
        </div>
        
        <div class="time-tracker">
          <div class="time-display text-center mb-4">
            <div class="text-h2 font-weight-bold text-primary">
              {{ timeDisplay }}
            </div>
            <p class="text-caption text-medium-emphasis">
              وقت العمل اليوم
            </p>
          </div>
          
          <div class="time-controls d-flex justify-center ga-2">
            <v-btn
              :color="isRunning ? 'warning' : 'success'"
              variant="flat"
              size="small"
              @click="toggleTimer"
            >
              <span class="me-1">{{ isRunning ? '⏸️' : '▶️' }}</span>
              {{ isRunning ? 'إيقاف' : 'تشغيل' }}
            </v-btn>
            
            <v-btn
              color="error"
              variant="flat"
              size="small"
              @click="stopTimer"
            >
              <span class="me-1">⏹️</span>
              إيقاف
            </v-btn>
          </div>
        </div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const teamMembers = ref([
  {
    id: 1,
    name: 'أحمد محمد',
    initials: 'أم',
    avatarColor: 'blue',
    task: 'العمل على مستودع مشروع GitHub',
    status: 'مكتمل',
    statusColor: 'success'
  },
  {
    id: 2,
    name: 'فاطمة علي',
    initials: 'فع',
    avatarColor: 'green',
    task: 'العمل على نظام مصادقة المستخدمين',
    status: 'قيد التنفيذ',
    statusColor: 'warning'
  },
  {
    id: 3,
    name: 'محمد حسن',
    initials: 'مح',
    avatarColor: 'purple',
    task: 'تطوير وظائف البحث والتصفية',
    status: 'معلق',
    statusColor: 'error'
  },
  {
    id: 4,
    name: 'نور الدين',
    initials: 'ند',
    avatarColor: 'orange',
    task: 'التخطيط المتجاوب للصفحة الرئيسية',
    status: 'قيد التنفيذ',
    statusColor: 'warning'
  }
])

const isRunning = ref(false)
const elapsedTime = ref(0)
let timerInterval = null

const timeDisplay = computed(() => {
  const hours = Math.floor(elapsedTime.value / 3600)
  const minutes = Math.floor((elapsedTime.value % 3600) / 60)
  const seconds = elapsedTime.value % 60
  
  return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

const toggleTimer = () => {
  isRunning.value = !isRunning.value
  
  if (isRunning.value) {
    timerInterval = setInterval(() => {
      elapsedTime.value++
    }, 1000)
  } else {
    clearInterval(timerInterval)
  }
}

const stopTimer = () => {
  isRunning.value = false
  elapsedTime.value = 0
  clearInterval(timerInterval)
}

onMounted(() => {
  // Initialize with some elapsed time for demo
  elapsedTime.value = 5048 // 1:24:08
})

onUnmounted(() => {
  if (timerInterval) {
    clearInterval(timerInterval)
  }
})
</script>

<style scoped>
.team-member {
  padding: 12px;
  border-radius: 8px;
  transition: background 0.2s ease;
}

.team-member:hover {
  background: #f5f5f5;
}

.donut-chart-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.donut-chart {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: conic-gradient(
    #4caf50 0deg 147.6deg,
    #8bc34a 147.6deg 180deg,
    #e0e0e0 180deg 360deg
  );
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.donut-chart::before {
  content: '';
  width: 60px;
  height: 60px;
  background: white;
  border-radius: 50%;
  position: absolute;
}

.donut-center {
  z-index: 1;
  text-align: center;
}

.chart-legend {
  width: 100%;
}

.legend-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.legend-dot.completed {
  background: #4caf50;
}

.legend-dot.in-progress {
  background: #8bc34a;
}

.legend-dot.pending {
  background: #e0e0e0;
}

.time-tracker {
  text-align: center;
}

.time-display {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 12px;
  margin-bottom: 20px;
}
</style>
