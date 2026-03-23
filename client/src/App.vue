<template>
  <div class="app-layout">
    <!-- Sidebar -->
    <aside class="sidebar" :class="{ 'sidebar-collapsed': sidebarCollapsed }">
      <div class="sidebar-header">
        <div class="sidebar-logo">
          <span class="sidebar-logo-name">{{ t('nav.companyName') }}</span>
          <span class="sidebar-logo-sub">{{ t('nav.subtitle') }}</span>
        </div>
        <button class="sidebar-toggle" @click="toggleSidebar" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path v-if="!sidebarCollapsed" d="M10 12L6 8L10 4" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path v-else d="M6 4L10 8L6 12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <nav class="sidebar-nav">
        <router-link
          to="/"
          class="sidebar-nav-item"
          :class="{ active: $route.path === '/' }"
          data-tooltip="Overview"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <rect x="2" y="2" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="2" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="11" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="11" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>

        <router-link
          to="/inventory"
          class="sidebar-nav-item"
          :class="{ active: $route.path === '/inventory' }"
          data-tooltip="Inventory"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M3 6L10 2L17 6V14L10 18L3 14V6Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M10 2V18M3 6L10 10L17 6" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link
          to="/orders"
          class="sidebar-nav-item"
          :class="{ active: $route.path === '/orders' }"
          data-tooltip="Orders"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M4 4H16V14C16 15.1 15.1 16 14 16H6C4.9 16 4 15.1 4 14V4Z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M2 4H18" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M8 4V3C8 2.45 8.45 2 9 2H11C11.55 2 12 2.45 12 3V4" stroke="currentColor" stroke-width="1.5"/>
            <path d="M8 9H12M8 12H11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>

        <router-link
          to="/spending"
          class="sidebar-nav-item"
          :class="{ active: $route.path === '/spending' }"
          data-tooltip="Finance"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <circle cx="10" cy="10" r="8" stroke="currentColor" stroke-width="1.5"/>
            <path d="M10 6V7M10 13V14M7.5 9C7.5 7.9 8.6 7 10 7C11.4 7 12.5 7.9 12.5 9C12.5 10.1 11.4 10.7 10 11C8.6 11.3 7.5 12 7.5 13C7.5 14.1 8.6 15 10 15C11.4 15 12.5 14.1 12.5 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>

        <router-link
          to="/demand"
          class="sidebar-nav-item"
          :class="{ active: $route.path === '/demand' }"
          data-tooltip="Demand Forecast"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M3 15L8 9L12 12L17 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M14 5H17V8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link
          to="/reports"
          class="sidebar-nav-item"
          :class="{ active: $route.path === '/reports' }"
          data-tooltip="Reports"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M5 3H15C15.55 3 16 3.45 16 4V16C16 16.55 15.55 17 15 17H5C4.45 17 4 16.55 4 16V4C4 3.45 4.45 3 5 3Z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7 7H13M7 10H13M7 13H10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">Reports</span>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- Main body -->
    <div class="app-body" :class="{ 'sidebar-collapsed': sidebarCollapsed }">
      <div class="topbar">
        <FilterBar />
      </div>
      <main class="page-content">
        <router-view />
      </main>
    </div>

    <!-- Modals (unchanged) -->
    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)

    const sidebarCollapsed = ref(false)

    const toggleSidebar = () => {
      sidebarCollapsed.value = !sidebarCollapsed.value
      localStorage.setItem('sidebar-collapsed', String(sidebarCollapsed.value))
    }
    const apiTasks = ref([])

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(() => {
      loadTasks()
      sidebarCollapsed.value = localStorage.getItem('sidebar-collapsed') === 'true'
    })

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      sidebarCollapsed,
      toggleSidebar
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f1f5f9;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ── Layout Shell ── */
.app-layout {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
}

/* ── Sidebar ── */
.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  width: 240px;
  background: #ffffff;
  border-right: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  z-index: 100;
  transition: width 0.2s ease;
  overflow: hidden;
}

.sidebar.sidebar-collapsed {
  width: 56px;
}

.sidebar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 0.875rem;
  min-height: 60px;
  border-bottom: 1px solid #e2e8f0;
  flex-shrink: 0;
}

.sidebar-logo {
  display: flex;
  flex-direction: column;
  gap: 2px;
  overflow: hidden;
  white-space: nowrap;
}

.sidebar-logo-name {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.02em;
}

.sidebar-logo-sub {
  font-size: 0.6875rem;
  color: #64748b;
  font-weight: 400;
}

.sidebar-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  background: transparent;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  color: #94a3b8;
  cursor: pointer;
  flex-shrink: 0;
  transition: all 0.15s ease;
}

.sidebar-toggle:hover {
  background: #f1f5f9;
  color: #0f172a;
  border-color: #cbd5e1;
}

/* Hide logo text when collapsed */
.sidebar-collapsed .sidebar-logo-name,
.sidebar-collapsed .sidebar-logo-sub {
  display: none;
}

/* ── Sidebar Nav ── */
.sidebar-nav {
  flex: 1;
  padding: 0.5rem 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 2px;
  overflow-y: auto;
  overflow-x: hidden;
}

.sidebar-nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.5625rem 0.75rem;
  border-radius: 7px;
  color: #64748b;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  white-space: nowrap;
  transition: background 0.15s ease, color 0.15s ease;
  position: relative;
}

.sidebar-nav-item:hover {
  background: #f1f5f9;
  color: #0f172a;
}

.sidebar-nav-item:hover .nav-icon {
  color: #0f172a;
}

.sidebar-nav-item.active {
  background: #eff6ff;
  color: #2563eb;
}

.sidebar-nav-item.active .nav-icon {
  color: #2563eb;
}

.nav-icon {
  color: #94a3b8;
  flex-shrink: 0;
  transition: color 0.15s ease;
}

.nav-label {
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Collapsed nav — icon-only + tooltip */
.sidebar-collapsed .nav-label {
  display: none;
}

.sidebar-collapsed .sidebar-nav-item {
  justify-content: center;
  padding: 0.5625rem;
}

.sidebar-collapsed .sidebar-nav-item:hover::after {
  content: attr(data-tooltip);
  position: absolute;
  left: calc(100% + 8px);
  top: 50%;
  transform: translateY(-50%);
  background: #1e293b;
  color: #f8fafc;
  padding: 4px 8px;
  border-radius: 5px;
  font-size: 12px;
  font-weight: 500;
  white-space: nowrap;
  z-index: 200;
  pointer-events: none;
}

/* ── Sidebar Footer ── */
.sidebar-footer {
  padding: 0.75rem 0.5rem;
  border-top: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 4px;
  flex-shrink: 0;
}

/* Collapsed footer — icon-only for LanguageSwitcher + ProfileMenu */
.sidebar-collapsed .sidebar-footer .language-button .language-label,
.sidebar-collapsed .sidebar-footer .language-button .chevron,
.sidebar-collapsed .sidebar-footer .profile-button .profile-name,
.sidebar-collapsed .sidebar-footer .profile-button .chevron {
  display: none;
}

.sidebar-collapsed .sidebar-footer .language-button,
.sidebar-collapsed .sidebar-footer .profile-button {
  justify-content: center;
  padding: 0.5rem;
  width: 100%;
}

/* Footer dropdowns open to the right when collapsed */
.sidebar-collapsed .sidebar-footer .dropdown-menu {
  left: calc(100% + 8px);
  top: auto;
  bottom: 0;
  right: auto;
}

/* ── App Body ── */
.app-body {
  margin-left: 240px;
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  transition: margin-left 0.2s ease;
}

.app-body.sidebar-collapsed {
  margin-left: 56px;
}

/* ── Topbar ── */
.topbar {
  position: sticky;
  top: 0;
  z-index: 90;
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
}

/* ── Page Content ── */
.page-content {
  flex: 1;
  padding: 1.5rem 2rem;
  background: #f1f5f9;
}

/* ── Global UI Tokens (unchanged from original) ── */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value { color: #ea580c; }
.stat-card.success .stat-value { color: #059669; }
.stat-card.danger .stat-value  { color: #dc2626; }
.stat-card.info .stat-value    { color: #2563eb; }

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success    { background: #d1fae5; color: #065f46; }
.badge.warning    { background: #fed7aa; color: #92400e; }
.badge.danger     { background: #fecaca; color: #991b1b; }
.badge.info       { background: #dbeafe; color: #1e40af; }
.badge.increasing { background: #d1fae5; color: #065f46; }
.badge.decreasing { background: #fecaca; color: #991b1b; }
.badge.stable     { background: #e0e7ff; color: #3730a3; }
.badge.high       { background: #fecaca; color: #991b1b; }
.badge.medium     { background: #fed7aa; color: #92400e; }
.badge.low        { background: #dbeafe; color: #1e40af; }

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
</style>
