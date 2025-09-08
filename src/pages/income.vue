<template>
  <v-container class="fill-height" max-width="1200">
    <div>
      <!-- Header Section -->
      <div class="text-center mb-8">
        <span class="mb-4" style="font-size: 64px; color: #4caf50;">📈</span>
        <h1 class="text-h3 font-weight-bold text-success mb-2">الإيرادات</h1>
        <p class="text-h6 text-medium-emphasis">إدارة وتتبع جميع مصادر الإيرادات</p>
      </div>

      <!-- Summary Cards -->
      <v-row class="mb-6">
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="success" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #4caf50;">💵</span>
            <h3 class="text-h4 font-weight-bold">{{ formatCurrency(totalIncome) }}</h3>
            <p class="text-subtitle-1">إجمالي الإيرادات</p>
          </v-card>
        </v-col>
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="info" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #2196f3;">📅</span>
            <h3 class="text-h4 font-weight-bold">{{ formatCurrency(monthlyIncome) }}</h3>
            <p class="text-subtitle-1">إيرادات هذا الشهر</p>
          </v-card>
        </v-col>
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="warning" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #ff9800;">📊</span>
            <h3 class="text-h4 font-weight-bold">{{ incomeGrowth }}%</h3>
            <p class="text-subtitle-1">نمو الإيرادات</p>
          </v-card>
        </v-col>
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="primary" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #9c27b0;">💰</span>
            <h3 class="text-h4 font-weight-bold">{{ incomeSources.length }}</h3>
            <p class="text-subtitle-1">مصادر الإيرادات</p>
          </v-card>
        </v-col>
      </v-row>

      <!-- Add Income Button -->
      <div class="text-center mb-6">
        <v-btn
          color="success"
          size="large"
          @click="showAddDialog = true"
        >
          <span class="me-2">➕</span>
          إضافة إيراد جديد
        </v-btn>
      </div>

      <!-- Income Sources Table -->
      <v-card class="mb-6">
        <v-card-title class="d-flex align-center">
          <v-icon class="me-2">mdi-format-list-bulleted</v-icon>
          مصادر الإيرادات
        </v-card-title>
        <v-data-table
          :headers="headers"
          :items="incomeSources"
          :loading="loading"
          class="elevation-1"
        >
          <template v-slot:item.amount="{ item }">
            <span class="font-weight-bold text-success">{{ formatCurrency(item.amount) }}</span>
          </template>
          <template v-slot:item.date="{ item }">
            {{ formatDate(item.date) }}
          </template>
          <template v-slot:item.actions="{ item }">
            <v-btn
              size="small"
              color="primary"
              @click="editIncome(item)"
            >
              <i class="mdi mdi-pencil"></i>
            </v-btn>
            <v-btn
              size="small"
              color="error"
              @click="deleteIncome(item)"
            >
              <i class="mdi mdi-delete"></i>
            </v-btn>
          </template>
        </v-data-table>
      </v-card>

      <!-- Add/Edit Income Dialog -->
      <v-dialog v-model="showAddDialog" max-width="600">
        <v-card>
          <v-card-title>
            <span class="text-h5">{{ editingIncome ? 'تعديل الإيراد' : 'إضافة إيراد جديد' }}</span>
          </v-card-title>
          <v-card-text>
            <v-form ref="form" v-model="valid">
              <v-text-field
                v-model="incomeForm.description"
                label="وصف الإيراد"
                :rules="[v => !!v || 'الوصف مطلوب']"
                required
              />
              <v-text-field
                v-model.number="incomeForm.amount"
                label="المبلغ"
                type="number"
                :rules="[v => v > 0 || 'المبلغ يجب أن يكون أكبر من صفر']"
                required
              />
              <v-select
                v-model="incomeForm.category"
                :items="incomeCategories"
                label="الفئة"
                :rules="[v => !!v || 'الفئة مطلوبة']"
                required
              />
              <v-textarea
                v-model="incomeForm.notes"
                label="ملاحظات"
                rows="3"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn color="grey" @click="closeDialog">إلغاء</v-btn>
            <v-btn color="success" @click="saveIncome" :disabled="!valid">حفظ</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </v-container>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

// Reactive data
const loading = ref(false)
const showAddDialog = ref(false)
const valid = ref(false)
const editingIncome = ref(null)

const incomeForm = ref({
  description: '',
  amount: 0,
  category: '',
  notes: ''
})

const incomeSources = ref([
  {
    id: 1,
    description: 'رسوم التسجيل',
    amount: 50000,
    category: 'رسوم طلابية',
    date: '2024-01-15',
    notes: 'رسوم الفصل الدراسي الأول'
  },
  {
    id: 2,
    description: 'منحة حكومية',
    amount: 100000,
    category: 'منح',
    date: '2024-01-10',
    notes: 'منحة وزارة التعليم العالي'
  },
  {
    id: 3,
    description: 'استشارات أكاديمية',
    amount: 25000,
    category: 'خدمات',
    date: '2024-01-05',
    notes: 'استشارات للقطاع الخاص'
  }
])

const incomeCategories = [
  'رسوم طلابية',
  'منح',
  'استثمارات',
  'خدمات',
  'تبرعات',
  'أخرى'
]

const headers = [
  { title: 'الوصف', key: 'description', align: 'start' },
  { title: 'المبلغ', key: 'amount', align: 'center' },
  { title: 'الفئة', key: 'category', align: 'center' },
  { title: 'التاريخ', key: 'date', align: 'center' },
  { title: 'الإجراءات', key: 'actions', align: 'center', sortable: false }
]

// Computed properties
const totalIncome = computed(() => {
  return incomeSources.value.reduce((sum, item) => sum + item.amount, 0)
})

const monthlyIncome = computed(() => {
  const currentMonth = new Date().getMonth()
  const currentYear = new Date().getFullYear()
  
  return incomeSources.value
    .filter(item => {
      const itemDate = new Date(item.date)
      return itemDate.getMonth() === currentMonth && itemDate.getFullYear() === currentYear
    })
    .reduce((sum, item) => sum + item.amount, 0)
})

const incomeGrowth = computed(() => {
  // Simplified growth calculation
  return 15.5
})

// Methods
const formatCurrency = (amount) => {
  return new Intl.NumberFormat('ar-SA', {
    style: 'currency',
    currency: 'SAR'
  }).format(amount)
}

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString('ar-SA')
}

const editIncome = (item) => {
  editingIncome.value = item
  incomeForm.value = { ...item }
  showAddDialog.value = true
}

const deleteIncome = (item) => {
  if (confirm('هل أنت متأكد من حذف هذا الإيراد؟')) {
    const index = incomeSources.value.findIndex(i => i.id === item.id)
    if (index > -1) {
      incomeSources.value.splice(index, 1)
    }
  }
}

const saveIncome = () => {
  if (valid.value) {
    if (editingIncome.value) {
      // Update existing income
      const index = incomeSources.value.findIndex(i => i.id === editingIncome.value.id)
      if (index > -1) {
        incomeSources.value[index] = {
          ...incomeForm.value,
          id: editingIncome.value.id,
          date: editingIncome.value.date
        }
      }
    } else {
      // Add new income
      const newIncome = {
        ...incomeForm.value,
        id: Date.now(),
        date: new Date().toISOString().split('T')[0]
      }
      incomeSources.value.unshift(newIncome)
    }
    closeDialog()
  }
}

const closeDialog = () => {
  showAddDialog.value = false
  editingIncome.value = null
  incomeForm.value = {
    description: '',
    amount: 0,
    category: '',
    notes: ''
  }
}

onMounted(() => {
  // Load data from API or localStorage
  loading.value = true
  setTimeout(() => {
    loading.value = false
  }, 1000)
})
</script>

<style scoped>
.v-card {
  border-radius: 12px;
}
</style>
