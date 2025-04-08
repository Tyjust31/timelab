<script lang="ts">
    // Props pour personnaliser le dashboard
    let sidebarWidth = "250px";
  let sidebarBgColor = "#1e293b";
   let textColor = "#e2e8f0";
   let accentColor = "#3b82f6";
   let appName = "Dashboard";
    import type { Snippet } from 'svelte';

    // État pour le toggle du sidebar sur mobile
    let sidebarOpen = $state(false);
    let innerWidth: number = $state();
    let { children, data }: { children: Snippet; data: any } = $props();

    // Navigation items
    const navItems = [
      { icon: "grid", label: "Dashboard", path: "/dashboard", active: true },
      { icon: "users", label: "Utilisateurs", path: "/users", active: false },
      { icon: "file-text", label: "Rapports", path: "/reports", active: false },
      { icon: "bar-chart", label: "Statistiques", path: "/stats", active: false },
      { icon: "settings", label: "Paramètres", path: "/settings", active: false }
    ];
    
    // Toggle sidebar on mobile
    function toggleSidebar() {
      sidebarOpen = !sidebarOpen;
    }
    
    // Handle window resize
    function handleResize() {
      if (innerWidth > 768) {
        sidebarOpen = true;
      } else {
        sidebarOpen = false;
      }
    }
  </script>
  
  <svelte:window bind:innerWidth={innerWidth} onresize={handleResize}/>
  
  <div class="dashboard-container">
    <!-- Sidebar -->
    <aside class="sidebar" class:open={sidebarOpen} style="--sidebar-width: {sidebarWidth}; --sidebar-bg: {sidebarBgColor}; --text-color: {textColor}; --accent-color: {accentColor};">
      <div class="sidebar-header">
        <h2>{appName}</h2>
        <button class="sidebar-toggle" on:click={toggleSidebar}>
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
      </div>
      
      <nav class="sidebar-nav">
        <ul>
          {#each navItems as item}
            <li class:active={item.active}>
              <a href={item.path}>
                <span class="icon">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    {#if item.icon === 'grid'}
                      <rect x="3" y="3" width="7" height="7"></rect>
                      <rect x="14" y="3" width="7" height="7"></rect>
                      <rect x="14" y="14" width="7" height="7"></rect>
                      <rect x="3" y="14" width="7" height="7"></rect>
                    {:else if item.icon === 'users'}
                      <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                      <circle cx="9" cy="7" r="4"></circle>
                      <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                      <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                    {:else if item.icon === 'file-text'}
                      <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                      <polyline points="14 2 14 8 20 8"></polyline>
                      <line x1="16" y1="13" x2="8" y2="13"></line>
                      <line x1="16" y1="17" x2="8" y2="17"></line>
                      <polyline points="10 9 9 9 8 9"></polyline>
                    {:else if item.icon === 'bar-chart'}
                      <line x1="18" y1="20" x2="18" y2="10"></line>
                      <line x1="12" y1="20" x2="12" y2="4"></line>
                      <line x1="6" y1="20" x2="6" y2="14"></line>
                    {:else if item.icon === 'settings'}
                      <circle cx="12" cy="12" r="3"></circle>
                      <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
                    {/if}
                  </svg>
                </span>
                <span class="label">{item.label}</span>
              </a>
            </li>
          {/each}
        </ul>
      </nav>
      
      <div class="sidebar-footer">
        <div class="user-info">
          <div class="avatar">
            <span>JD</span>
          </div>
          <div class="user-details">
            <p class="user-name">John Doe</p>
            <p class="user-role">Administrateur</p>
          </div>
        </div>
      </div>
    </aside>
    
    <!-- Main Content Area -->
    <main class="content">
      <!-- Mobile header with toggle button -->
      <header class="mobile-header">
        <button class="sidebar-toggle" on:click={toggleSidebar}>
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="3" y1="12" x2="21" y2="12"></line>
            <line x1="3" y1="6" x2="21" y2="6"></line>
            <line x1="3" y1="18" x2="21" y2="18"></line>
          </svg>
        </button>
        <h2>{appName}</h2>
      </header>
      
      <!-- Slot for page content -->
      <div class="page-content">
        {@render children()}
      </div>
    </main>
  </div>
  
  <style>
    /* Dashboard Layout */
    .dashboard-container {
      display: flex;
      min-height: 100vh;
      width: 100%;
      position: relative;
    }
    
    /* Sidebar Styles */
    .sidebar {
      width: var(--sidebar-width);
      background-color: var(--sidebar-bg);
      color: var(--text-color);
      height: 100vh;
      position: fixed;
      left: 0;
      top: 0;
      z-index: 10;
      display: flex;
      flex-direction: column;
      transition: transform 0.3s ease;
    }
    
    .sidebar-header {
      padding: 1.5rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .sidebar-header h2 {
      margin: 0;
      font-size: 1.5rem;
      font-weight: 600;
    }
    
    .sidebar-nav {
      flex: 1;
      padding: 1rem 0;
      overflow-y: auto;
    }
    
    .sidebar-nav ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }
    
    .sidebar-nav li {
      margin-bottom: 0.25rem;
    }
    
    .sidebar-nav li a {
      display: flex;
      align-items: center;
      padding: 0.75rem 1.5rem;
      color: var(--text-color);
      text-decoration: none;
      transition: background-color 0.2s;
      border-radius: 0.25rem;
      margin: 0 0.5rem;
    }
    
    .sidebar-nav li a:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }
    
    .sidebar-nav li.active a {
      background-color: var(--accent-color);
      color: white;
    }
    
    .sidebar-nav .icon {
      margin-right: 0.75rem;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 24px;
    }
    
    .sidebar-footer {
      padding: 1rem 1.5rem;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .user-info {
      display: flex;
      align-items: center;
    }
    
    .avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background-color: var(--accent-color);
      display: flex;
      align-items: center;
      justify-content: center;
      margin-right: 0.75rem;
      font-weight: 600;
    }
    
    .user-details {
      overflow: hidden;
    }
    
    .user-name {
      margin: 0;
      font-weight: 500;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    
    .user-role {
      margin: 0;
      font-size: 0.8rem;
      opacity: 0.8;
    }
    
    /* Content Area */
    .content {
      flex: 1;
      margin-left: var(--sidebar-width);
      transition: margin-left 0.3s ease;
    }
    
    .page-content {
      padding: 2rem;
    }
    
    /* Mobile Header */
    .mobile-header {
      display: none;
      padding: 1rem;
      background-color: white;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
      align-items: center;
    }
    
    .mobile-header h2 {
      margin: 0 0 0 1rem;
      font-size: 1.25rem;
    }
    
    .sidebar-toggle {
      background: none;
      border: none;
      color: inherit;
      cursor: pointer;
      padding: 0.5rem;
      border-radius: 0.25rem;
      display: none;
      align-items: center;
      justify-content: center;
    }
    
    .sidebar-toggle:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }
    
    /* Responsive Adjustments */
    @media (max-width: 768px) {
      .sidebar {
        transform: translateX(-100%);
      }
      
      .sidebar.open {
        transform: translateX(0);
      }
      
      .content {
        margin-left: 0;
      }
      
      .mobile-header {
        display: flex;
      }
      
      .sidebar-toggle {
        display: flex;
      }
      
      .page-content {
        padding: 1rem;
      }
    }
  </style>