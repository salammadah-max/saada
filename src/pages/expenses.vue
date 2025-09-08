<template>
  <v-container class="fill-height" max-width="1200">
    <div>
      <!-- Header Section -->
      <div class="text-center mb-8">
        <span class="mb-4" style="font-size: 64px; color: #f44336;">📉</span>
        <h1 class="text-h3 font-weight-bold text-error mb-2">المصروفات</h1>
        <p class="text-h6 text-medium-emphasis">إدارة وتتبع جميع المصروفات والنفقات</p>
      </div>

      <!-- Summary Cards -->
      <v-row class="mb-6">
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="error" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #f44336;">💸</span>
            <h3 class="text-h4 font-weight-bold">{{ formatCurrency(totalExpenses) }}</h3>
            <p class="text-subtitle-1">إجمالي المصروفات</p>
          </v-card>
        </v-col>
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="info" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #2196f3;">📅</span>
            <h3 class="text-h4 font-weight-bold">{{ formatCurrency(monthlyExpenses) }}</h3>
            <p class="text-subtitle-1">مصروفات هذا الشهر</p>
          </v-card>
        </v-col>
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="warning" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #ff9800;">📊</span>
            <h3 class="text-h4 font-weight-bold">{{ expenseGrowth }}%</h3>
            <p class="text-subtitle-1">نمو المصروفات</p>
          </v-card>
        </v-col>
        <v-col cols="12" md="3">
          <v-card class="text-center pa-4" color="primary" variant="tonal">
            <span class="mb-2" style="font-size: 48px; color: #9c27b0;">🧾</span>
            <h3 class="text-h4 font-weight-bold">{{ expenses.length }}</h3>
            <p class="text-subtitle-1">عدد المصروفات</p>
          </v-card>
        </v-col>
      </v-row>

      <!-- Add Expense Button -->
      <div class="text-center mb-6">
        <v-btn
          color="error"
          size="large"
          @click="showAddDialog = true"
        >
          <span class="me-2">➕</span>
          إضافة مصروف جديد
        </v-btn>
      </div>

      <!-- Expenses Table -->
      <v-card class="mb-6">
        <v-card-title class="d-flex align-center">
          <v-icon class="me-2">mdi-format-list-bulleted</v-icon>
          قائمة المصروفات
        </v-card-title>
        <v-data-table
          :headers="headers"
          :items="expenses"
          :loading="loading"
          class="elevation-1"
        >
          <template v-slot:item.amount="{ item }">
            <span class="font-weight-bold text-error">{{ formatCurrency(item.amount) }}</span>
          </template>
          <template v-slot:item.date="{ item }">
            {{ formatDate(item.date) }}
          </template>
          <template v-slot:item.status="{ item }">
            <v-chip
              :color="getStatusColor(item.status)"
              size="small"
            >
              {{ getStatusText(item.status) }}
            </v-chip>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-btn
              size="small"
              color="primary"
              @click="editExpense(item)"
            >
              <i class="mdi mdi-pencil"></i>
            </v-btn>
            <v-btn
              size="small"
              color="error"
              @click="deleteExpense(item)"
            >
              <i class="mdi mdi-delete"></i>
            </v-btn>
          </template>
        </v-data-table>
      </v-card>

      <!-- Add/Edit Expense Dialog -->
      <v-dialog v-model="showAddDialog" max-width="600">
        <v-card>
          <v-card-title>
            <span class="text-h5">{{ editingExpense ? 'تعديل المصروف' : 'إضافة مصروف جديد' }}</span>
          </v-card-title>
          <v-card-text>
            <v-form ref="form" v-model="valid">
              <v-text-field
                v-model="expenseForm.description"
                label="وصف المصروف"
                :rules="[v => !!v || 'الوصف مطلوب']"
                required
              />
              <v-text-field
                v-model.number="expenseForm.amount"
                label="المبلغ"
                type="number"
                :rules="[v => v > 0 || 'المبلغ يجب أن يكون أكبر من صفر']"
                required
              />
              <v-select
                v-model="expenseForm.category"
                :items="expenseCategories"
                label="الفئة"
                :rules="[v => !!v || 'الفئة مطلوبة']"
                required
              />
              <v-select
                v-model="expenseForm.status"
                :items="statusOptions"
                label="الحالة"
                :rules="[v => !!v || 'الحالة مطلوبة']"
                required
              />
              <v-textarea
                v-model="expenseForm.notes"
                label="ملاحظات"
                rows="3"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn color="grey" @click="closeDialog">إلغاء</v-btn>
            <v-btn color="error" @click="saveExpense" :disabled="!valid">حفظ</v-btn>
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
const editingExpense = ref(null)

const expenseForm = ref({
  description: '',
  amount: 0,
  category: '',
  status: 'pending',
  notes: ''
})

const expenses = ref([
  {
    id: 1,
    description: 'راتب الموظفين',
    amount: 75000,
    category: 'رواتب',
    status: 'approved',
    date: '2024-01-15',
    notes: 'رواتب شهر يناير'
  },
  {
    id: 2,
    description: 'فاتورة الكهرباء',
    amount: 5000,
    category: 'مرافق',
    status: 'pending',
    date: '2024-01-12',
    notes: 'فاتورة شهر ديسمبر'
  },
  {
    id: 3,
    description: 'صيانة المبنى',
    amount: 15000,
    category: 'صيانة',
    status: 'approved',
    date: '2024-01-08',
    notes: 'صيانة دورية للمبنى الرئيسي'
  },
  {
    id: 4,
    description: 'معدات مكتبية',
    amount: 8000,
    category: 'معدات',
    status: 'rejected',
    date: '2024-01-05',
    notes: 'شراء أجهزة حاسوب جديدة'
  }
])

const expenseCategories = [
  'رواتب',
  'مرافق',
  'صيانة',
  'معدات',
  'تدريب',
  'سفر',
  'أخرى'
]

const statusOptions = [
  { value: 'pending', title: 'معلق' },
  { value: 'approved', title: 'موافق عليه' },
  { value: 'rejected', title: 'مرفوض' }
]

const headers = [
  { title: 'الوصف', key: 'description', align: 'start' },
  { title: 'المبلغ', key: 'amount', align: 'center' },
  { title: 'الفئة', key: 'category', align: 'center' },
  { title: 'الحالة', key: 'status', align: 'center' },
  { title: 'التاريخ', key: 'date', align: 'center' },
  { title: 'الإجراءات', key: 'actions', align: 'center', sortable: false }
]

// Computed properties
const totalExpenses = computed(() => {
  return expenses.value.reduce((sum, item) => sum + item.amount, 0)
})

const monthlyExpenses = computed(() => {
  const currentMonth = new Date().getMonth()
  const currentYear = new Date().getFullYear()
  
  return expenses.value
    .filter(item => {
      const itemDate = new Date(item.date)
      return itemDate.getMonth() === currentMonth && itemDate.getFullYear() === currentYear
    })
    .reduce((sum, item) => sum + item.amount, 0)
})

const expenseGrowth = computed(() => {
  // Simplified growth calculation
  return 8.2
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

const getStatusColor = (status) => {
  const colors = {
    pending: 'warning',
    approved: 'success',
    rejected: 'error'
  }
  return colors[status] || 'grey'
}

const getStatusText = (status) => {
  const texts = {
    pending: 'معلق',
    approved: 'موافق عليه',
    rejected: 'مرفوض'
  }
  return texts[status] || status
}

const editExpense = (item) => {
  editingExpense.value = item
  expenseForm.value = { ...item }
  showAddDialog.value = true
}

const deleteExpense = (item) => {
  if (confirm('هل أنت متأكد من حذف هذا المصروف؟')) {
    const index = expenses.value.findIndex(i => i.id === item.id)
    if (index > -1) {
      expenses.value.splice(index, 1)
    }
  }
}

const saveExpense = () => {
  if (valid.value) {
    if (editingExpense.value) {
      // Update existing expense
      const index = expenses.value.findIndex(i => i.id === editingExpense.value.id)
      if (index > -1) {
        expenses.value[index] = {
          ...expenseForm.value,
          id: editingExpense.value.id,
          date: editingExpense.value.date
        }
      }
    } else {
      // Add new expense
      const newExpense = {
        ...expenseForm.value,
        id: Date.now(),
        date: new Date().toISOString().split('T')[0]
      }
      expenses.value.unshift(newExpense)
    }
    closeDialog()
  }
}

const closeDialog = () => {
  showAddDialog.value = false
  editingExpense.value = null
  expenseForm.value = {
    description: '',
    amount: 0,
    category: '',
    status: 'pending',
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
