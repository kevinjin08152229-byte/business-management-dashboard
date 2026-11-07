<script setup lang="ts">
import { ref, computed } from 'vue'
import Sidebar from '../components/Sidebar.vue'
const searchQuery = ref('')
const selectedDepartment = ref('All')
const selectedStatus = ref('All')
const editingEmployeeId = ref<number | null>(null)
const newEmployee = ref({
  name: '',
  email: '',
  department: 'Engineering',
  position: '',
  status: 'Active' as 'Active' | 'On Leave',
})

const formError = ref('')
function editEmployee(employee: Employee) {
  editingEmployeeId.value = employee.id

  newEmployee.value = {
    name: employee.name,
    email: employee.email,
    department: employee.department,
    position: employee.position,
    status: employee.status,
  }
}
function updateEmployee() {
  if (
    !newEmployee.value.name.trim() ||
    !newEmployee.value.email.trim() ||
    !newEmployee.value.position.trim()
  ) {
    formError.value = 'Please complete all required fields.'
    return
  }

  const employee = employees.value.find(
    e => e.id === editingEmployeeId.value
  )

  if (!employee) return

  Object.assign(employee, newEmployee.value)

  editingEmployeeId.value = null
  formError.value = ''

  newEmployee.value = {
    name: '',
    email: '',
    department: 'Engineering',
    position: '',
    status: 'Active',
  }
}
function deleteEmployee(id: number) {
  const confirmed = window.confirm(
    'Are you sure you want to delete this employee?'
  )

  if (!confirmed) return

  employees.value = employees.value.filter(
    employee => employee.id !== id
  )

  if (editingEmployeeId.value === id) {
    editingEmployeeId.value = null
  }
}
function addEmployee() {
  if (
    !newEmployee.value.name.trim() ||
    !newEmployee.value.email.trim() ||
    !newEmployee.value.position.trim()
  ) {
    formError.value = 'Please complete all required fields.'
    return
  }

  if (!newEmployee.value.email.includes('@')) {
    formError.value = 'Please enter a valid email address.'
    return
  }

  employees.value.push({
    id: Math.max(0, ...employees.value.map(e => e.id)) + 1,
    name: newEmployee.value.name.trim(),
    email: newEmployee.value.email.trim(),
    department: newEmployee.value.department,
    position: newEmployee.value.position.trim(),
    status: newEmployee.value.status,
  })

  newEmployee.value = {
    name: '',
    email: '',
    department: 'Engineering',
    position: '',
    status: 'Active',
  }

  formError.value = ''
}
const filteredEmployees = computed(() => {
  return employees.value.filter(employee => {
    const matchesSearch = employee.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
                          employee.email.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesDepartment = selectedDepartment.value === 'All' || employee.department === selectedDepartment.value
    const matchesStatus = selectedStatus.value === 'All' || employee.status === selectedStatus.value

    return matchesSearch && matchesDepartment && matchesStatus
  })
})

interface Employee {
  id: number
  name: string
  email: string
  department: string
  position: string
  status: 'Active' | 'On Leave'
}

const employees = ref<Employee[]>([
  {
    id: 1,
    name: 'John Smith',
    email: 'john@example.com',
    department: 'Engineering',
    position: 'Frontend Developer',
    status: 'Active',
  },
  {
    id: 2,
    name: 'Emma Wilson',
    email: 'emma@example.com',
    department: 'Marketing',
    position: 'Marketing Manager',
    status: 'Active',
  },
  {
    id: 3,
    name: 'David Brown',
    email: 'david@example.com',
    department: 'Engineering',
    position: 'Backend Developer',
    status: 'On Leave',
  },
  {
    id: 4,
    name: 'Sophia Lee',
    email: 'sophia@example.com',
    department: 'Finance',
    position: 'Accountant',
    status: 'Active',
  },
])
</script>

<template>
  <div class="min-h-screen bg-slate-100">
    <div class="flex min-h-screen">
      <Sidebar />
      <main class="min-w-0 flex-1 p-6 md:p-8">
    
            <h1 class="text-3xl font-bold text-slate-800">
              Employees Management
            </h1>
        
            <p class="mt-2 text-slate-500">
              Manage your employees and departments.
            </p>
        
            <!-- Employee statistics -->
            <div class="mt-8 grid gap-6 md:grid-cols-3">
              <div class="rounded-2xl bg-white p-6 shadow-sm">
                <p class="text-sm text-slate-500">Total Employees</p>
                <h2 class="mt-3 text-3xl font-bold">
                 {{ filteredEmployees.length }}
                </h2>
              </div>
        
              <div class="rounded-2xl bg-white p-6 shadow-sm">
                <p class="text-sm text-slate-500">Active Employees</p>
                <h2 class="mt-3 text-3xl font-bold text-emerald-600">
                 {{ filteredEmployees.filter(e => e.status === 'Active').length }}
                </h2>
                </div>
        
              <div class="rounded-2xl bg-white p-6 shadow-sm">
                <p class="text-sm text-slate-500">On Leave</p>
                <h2 class="mt-3 text-3xl font-bold text-orange-500">
                  {{ filteredEmployees.filter(e => e.status === 'On Leave').length }}
                </h2>
              </div>
            </div>
        <div class="mt-8 rounded-2xl bg-white p-6 shadow-sm">
  <h2 class="mb-5 text-xl font-bold">Add New Employee</h2>

  <form
     @submit.prevent="editingEmployeeId !== null
    ? updateEmployee()
    : addEmployee()"
    class="grid gap-4 md:grid-cols-2"
  >
    <input
      v-model="newEmployee.name"
      placeholder="Employee name"
      class="rounded-lg border p-3"
    />

    <input
      v-model="newEmployee.email"
      type="email"
      placeholder="Email address"
      class="rounded-lg border p-3"
    />

    <input
      v-model="newEmployee.position"
      placeholder="Job position"
      class="rounded-lg border p-3"
    />

    <select
      v-model="newEmployee.department"
      class="rounded-lg border p-3"
    >
      <option value="Engineering">Engineering</option>
      <option value="Marketing">Marketing</option>
      <option value="Finance">Finance</option>
    </select>

    <select
      v-model="newEmployee.status"
      class="rounded-lg border p-3"
    >
      <option value="Active">Active</option>
      <option value="On Leave">On Leave</option>
    </select>

    <div class="flex items-center">
      <button
        type="submit"
        class="rounded-lg bg-blue-600 px-6 py-3 font-medium text-white transition hover:bg-blue-700"
      >
        {{ editingEmployeeId !== null ? 'Update Employee' : 'Add Employee' }}
      </button>
    </div>
  </form>

  <p v-if="formError" class="mt-3 text-sm text-red-600">
    {{ formError }}
  </p>
</div>
            <!-- Employee table -->
            <div class="mt-8 overflow-x-auto rounded-2xl bg-white p-6 shadow-sm">
             <div class="mb-6 flex flex-wrap items-center justify-between gap-4">
  <h2 class="text-xl font-bold">Employee List</h2>

  <div class="flex flex-wrap gap-3">
    <input
      v-model="searchQuery"
      type="text"
      placeholder="Search employees..."
      class="rounded-lg border border-slate-200 px-4 py-2 text-sm outline-none focus:border-blue-500"
    />

    <select
      v-model="selectedDepartment"
      class="rounded-lg border border-slate-200 px-4 py-2 text-sm"
    >
      <option value="All">All Departments</option>
      <option value="Engineering">Engineering</option>
      <option value="Marketing">Marketing</option>
      <option value="Finance">Finance</option>
    </select>

    <select
      v-model="selectedStatus"
      class="rounded-lg border border-slate-200 px-4 py-2 text-sm"
    >
      <option value="All">All Statuses</option>
      <option value="Active">Active</option>
      <option value="On Leave">On Leave</option>
    </select>
  </div>
</div>
        
              <table class="w-full text-left text-sm">
                <thead>
                  <tr class="border-b text-slate-500">
                    <th class="p-3">Name</th>
                    <th class="p-3">Department</th>
                    <th class="p-3">Position</th>
                    <th class="p-3">Status</th>
                    <th class="p-3">Actions</th>
                  </tr>
                </thead>
        
                <tbody>
                  <tr
                    v-for="employee in filteredEmployees"
                    :key="employee.id"
                    class="border-b last:border-0 hover:bg-slate-50"
                  >
                    <td class="p-3">
                      <p class="font-semibold">{{ employee.name }}</p>
                      <p class="text-xs text-slate-500">{{ employee.email }}</p>
                    </td>
        
                    <td class="p-3">{{ employee.department }}</td>
        
                    <td class="p-3">{{ employee.position }}</td>
        
                    <td class="p-3">
                      <span
                        class="rounded-full px-3 py-1 text-xs font-medium"
                        :class="employee.status === 'Active'
                          ? 'bg-emerald-100 text-emerald-700'
                          : 'bg-orange-100 text-orange-700'"
                      >
                        {{ employee.status }}
                      </span>
                    </td>
                    <td class="p-3">
                      <div class="flex gap-2">
                        <button
                          @click="editEmployee(employee)"
                          class="rounded-md bg-blue-500 px-3 py-1 text-sm text-white hover:bg-blue-600"
                        >
                          Edit
                        </button>
                        <button
                          @click="deleteEmployee(employee.id)"
                          class="rounded-md bg-red-500 px-3 py-1 text-sm text-white hover:bg-red-600"
                        >
                          Delete
                        </button>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
        </main>
    </div>
  </div>
</template>