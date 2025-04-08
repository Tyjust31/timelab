<script>
    import { onMount } from 'svelte';
    
    // Données simulées pour le dashboard
    let revenueData = [
      { month: 'Jan', amount: 42500 },
      { month: 'Fév', amount: 38700 },
      { month: 'Mar', amount: 45200 },
      { month: 'Avr', amount: 51300 },
      { month: 'Mai', amount: 49800 },
      { month: 'Juin', amount: 58600 }
    ];
    
    let expenseData = [
      { month: 'Jan', amount: 31200 },
      { month: 'Fév', amount: 29800 },
      { month: 'Mar', amount: 32400 },
      { month: 'Avr', amount: 35600 },
      { month: 'Mai', amount: 38100 },
      { month: 'Juin', amount: 41300 }
    ];
    
    // KPI Data
    let kpis = [
      { 
        title: "Chiffre d'affaires", 
        value: "58 600€", 
        trend: "+17.7%", 
        positive: true,
        icon: "trending-up"
      },
      { 
        title: "Dépenses", 
        value: "41 300€", 
        trend: "+8.4%", 
        positive: false,
        icon: "credit-card"
      },
      { 
        title: "Bénéfice net", 
        value: "17 300€", 
        trend: "+52.8%", 
        positive: true,
        icon: "dollar-sign"
      },
      { 
        title: "Factures impayées", 
        value: "12 450€", 
        trend: "-3.2%", 
        positive: true,
        icon: "file-text"
      }
    ];
    
    // Transactions récentes
    let recentTransactions = [
      { id: "INV-2025-04-123", client: "Tech Solutions SAS", date: "04/04/2025", montant: 4850, statut: "payée" },
      { id: "INV-2025-04-124", client: "Design Studio EURL", date: "03/04/2025", montant: 2300, statut: "en attente" },
      { id: "INV-2025-04-125", client: "Consulting Partners", date: "02/04/2025", montant: 7500, statut: "payée" },
      { id: "DEP-2025-04-042", client: "Loyer Bureau", date: "01/04/2025", montant: -2800, statut: "débitée" },
      { id: "DEP-2025-04-043", client: "Abonnements SaaS", date: "01/04/2025", montant: -650, statut: "débitée" }
    ];
    
    // Données pour le graphique
    let chartData = [];
    let chartCanvas;
    let chart;
    let selectedPeriod = "Mois";
    
    // Factures à payer
    let upcomingInvoices = [
      { fournisseur: "Fournisseur Internet", date: "15/04/2025", montant: 89 },
      { fournisseur: "Électricité", date: "18/04/2025", montant: 320 },
      { fournisseur: "Assurance", date: "22/04/2025", montant: 450 },
      { fournisseur: "Prestataire IT", date: "30/04/2025", montant: 1200 }
    ];
    
    // Nouvelles données pour différentes périodes
    const quarterlyData = {
      revenue: [
        { month: 'Q1', amount: 126400 },
        { month: 'Q2', amount: 159700 }
      ],
      expenses: [
        { month: 'Q1', amount: 93400 },
        { month: 'Q2', amount: 115000 }
      ]
    };
    
    const yearlyData = {
      revenue: [
        { month: '2024', amount: 286100 },
        { month: '2025 (en cours)', amount: 159700 }
      ],
      expenses: [
        { month: '2024', amount: 208400 },
        { month: '2025 (en cours)', amount: 115000 }
      ]
    };
    
    // Fonction pour changer la période
    function changePeriod(period) {
      selectedPeriod = period;
      
      // Mettre à jour les données selon la période sélectionnée
      if (period === "Mois") {
        updateChartData(revenueData, expenseData);
      } else if (period === "Trimestre") {
        updateChartData(quarterlyData.revenue, quarterlyData.expenses);
      } else if (period === "Année") {
        updateChartData(yearlyData.revenue, yearlyData.expenses);
      }
    }
    
    // Mise à jour des données du graphique
    function updateChartData(revenue, expenses) {
      chartData = {
        labels: revenue.map(d => d.month),
        revenues: revenue.map(d => d.amount),
        expenses: expenses.map(d => d.amount)
      };
    }
    
    // Nouvelle facture
    let showNewInvoiceModal = false;
    let newInvoice = {
      client: "",
      montant: "",
      date: ""
    };
    
    function toggleNewInvoiceModal() {
      showNewInvoiceModal = !showNewInvoiceModal;
      if (showNewInvoiceModal) {
        // Réinitialiser le formulaire
        newInvoice = {
          client: "",
          montant: "",
          date: ""
        };
      }
    }
    
    function addNewInvoice() {
      if (newInvoice.client && newInvoice.montant && newInvoice.date) {
        const today = new Date();
        const newId = `INV-${today.getFullYear()}-${String(today.getMonth() + 1).padStart(2, '0')}-${Math.floor(Math.random() * 1000)}`;
        
        recentTransactions = [{
          id: newId,
          client: newInvoice.client,
          date: newInvoice.date,
          montant: parseFloat(newInvoice.montant),
          statut: "en attente"
        }, ...recentTransactions];
        
        toggleNewInvoiceModal();
      }
    }
    
    // Nouvelle dépense
    let showNewExpenseModal = false;
    let newExpense = {
      description: "",
      montant: "",
      date: ""
    };
    
    function toggleNewExpenseModal() {
      showNewExpenseModal = !showNewExpenseModal;
      if (showNewExpenseModal) {
        // Réinitialiser le formulaire
        newExpense = {
          description: "",
          montant: "",
          date: ""
        };
      }
    }
    
    function addNewExpense() {
      if (newExpense.description && newExpense.montant && newExpense.date) {
        const today = new Date();
        const newId = `DEP-${today.getFullYear()}-${String(today.getMonth() + 1).padStart(2, '0')}-${Math.floor(Math.random() * 1000)}`;
        
        recentTransactions = [{
          id: newId,
          client: newExpense.description,
          date: newExpense.date,
          montant: -parseFloat(newExpense.montant),
          statut: "débitée"
        }, ...recentTransactions];
        
        toggleNewExpenseModal();
      }
    }
    
    // Fonctionnalité de recherche
    let searchQuery = "";
    let filteredTransactions = recentTransactions;
    
    function filterTransactions() {
      if (!searchQuery.trim()) {
        filteredTransactions = recentTransactions;
        return;
      }
      
      searchQuery = searchQuery.toLowerCase();
      filteredTransactions = recentTransactions.filter(transaction => 
        transaction.id.toLowerCase().includes(searchQuery) ||
        transaction.client.toLowerCase().includes(searchQuery) ||
        transaction.date.includes(searchQuery) ||
        transaction.statut.toLowerCase().includes(searchQuery)
      );
    }
    
    $: {
      searchQuery;
      filterTransactions();
    }
    
    // Fonction pour payer une facture
    function payInvoice(index) {
      upcomingInvoices = upcomingInvoices.filter((_, i) => i !== index);
      
      // On pourrait également l'ajouter aux transactions récentes
      const paid = upcomingInvoices[index];
      const today = new Date();
      const formattedDate = `${String(today.getDate()).padStart(2, '0')}/${String(today.getMonth() + 1).padStart(2, '0')}/${today.getFullYear()}`;
      
      recentTransactions = [{
        id: `DEP-${today.getFullYear()}-${String(today.getMonth() + 1).padStart(2, '0')}-${Math.floor(Math.random() * 1000)}`,
        client: paid.fournisseur,
        date: formattedDate,
        montant: -paid.montant,
        statut: "débitée"
      }, ...recentTransactions];
    }
    
    onMount(() => {
      // Initialiser les données du graphique
      updateChartData(revenueData, expenseData);
    });
    
    // Dates pour le filtre
    const currentDate = new Date();
    const currentMonth = currentDate.toLocaleString('default', { month: 'long' });
    const currentYear = currentDate.getFullYear();
    
    // Fonction de téléchargement du rapport
    function exportReport() {
      alert("Rapport financier téléchargé!");
      // Dans une application réelle, cela générerait un PDF ou un fichier Excel
    }
    
    // Calculs pour le tableau de bord
    function calculateProfitMargin() {
      return Math.round((17300 / 58600) * 100);
    }
    
    function calculateRevenueTrend() {
      const lastMonth = revenueData[revenueData.length - 1].amount;
      const previousMonth = revenueData[revenueData.length - 2].amount;
      return Math.round(((lastMonth - previousMonth) / previousMonth) * 100);
    }
  </script>
  
  <div class="dashboard">
    <!-- En-tête du Dashboard -->
    <header class="dashboard-header">
      <div>
        <h1>Tableau de bord financier</h1>
        <p class="subtitle">Vue d'ensemble de votre activité financière</p>
      </div>
      <div class="date-filter">
        <span class="period">{currentMonth} {currentYear}</span>
        <div class="filter-buttons">
          <button class="filter-btn {selectedPeriod === 'Mois' ? 'active' : ''}" on:click={() => changePeriod('Mois')}>Mois</button>
          <button class="filter-btn {selectedPeriod === 'Trimestre' ? 'active' : ''}" on:click={() => changePeriod('Trimestre')}>Trimestre</button>
          <button class="filter-btn {selectedPeriod === 'Année' ? 'active' : ''}" on:click={() => changePeriod('Année')}>Année</button>
        </div>
      </div>
    </header>
    
    <!-- KPIs principaux -->
    <div class="kpi-cards">
      {#each kpis as kpi}
        <div class="kpi-card">
          <div class="kpi-icon">
            {#if kpi.icon === 'trending-up'}
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="23 6 13.5 15.5 8.5 10.5 1 18"></polyline>
                <polyline points="17 6 23 6 23 12"></polyline>
              </svg>
            {:else if kpi.icon === 'credit-card'}
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="1" y="4" width="22" height="16" rx="2" ry="2"></rect>
                <line x1="1" y1="10" x2="23" y2="10"></line>
              </svg>
            {:else if kpi.icon === 'dollar-sign'}
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="12" y1="1" x2="12" y2="23"></line>
                <path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path>
              </svg>
            {:else if kpi.icon === 'file-text'}
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                <polyline points="14 2 14 8 20 8"></polyline>
                <line x1="16" y1="13" x2="8" y2="13"></line>
                <line x1="16" y1="17" x2="8" y2="17"></line>
                <polyline points="10 9 9 9 8 9"></polyline>
              </svg>
            {/if}
          </div>
          <div class="kpi-content">
            <p class="kpi-title">{kpi.title}</p>
            <p class="kpi-value">{kpi.value}</p>
            <p class="kpi-trend {kpi.positive ? 'positive' : 'negative'}">
              {kpi.trend}
              {#if kpi.positive}
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <polyline points="18 15 12 9 6 15"></polyline>
                </svg>
              {:else}
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <polyline points="6 9 12 15 18 9"></polyline>
                </svg>
              {/if}
            </p>
          </div>
        </div>
      {/each}
      
      <!-- Nouveaux KPIs -->
      <div class="kpi-card">
        <div class="kpi-icon">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M22 12h-4l-3 9L9 3l-3 9H2"></path>
          </svg>
        </div>
        <div class="kpi-content">
          <p class="kpi-title">Marge bénéficiaire</p>
          <p class="kpi-value">{calculateProfitMargin()}%</p>
          <p class="kpi-trend positive">
            +2.5%
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="18 15 12 9 6 15"></polyline>
            </svg>
          </p>
        </div>
      </div>
      
      <div class="kpi-card">
        <div class="kpi-icon">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M12 20v-6M6 20V10M18 20V4"></path>
          </svg>
        </div>
        <div class="kpi-content">
          <p class="kpi-title">Progression du CA</p>
          <p class="kpi-value">{calculateRevenueTrend()}%</p>
          <p class="kpi-trend positive">
            Ce mois-ci
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="18 15 12 9 6 15"></polyline>
            </svg>
          </p>
        </div>
      </div>
    </div>
    
    <!-- Graphiques et Tableaux -->
    <div class="dashboard-content">
      <!-- Graphique principal -->
      <div class="chart-container">
        <div class="chart-header">
          <h2>Revenus et Dépenses</h2>
          <div class="chart-legend">
            <div class="legend-item">
              <span class="legend-color revenue"></span>
              <span>Revenus</span>
            </div>
            <div class="legend-item">
              <span class="legend-color expense"></span>
              <span>Dépenses</span>
            </div>
          </div>
        </div>
        
        <!-- Représentation simplifiée du graphique -->
        <div class="chart-visual">
          <div class="chart-columns">
            {#if chartData.labels}
              {#each chartData.labels as label, i}
                <div class="chart-column">
                  <div class="chart-bar-container">
                    <div class="chart-bar revenue" style="height: {(chartData.revenues[i] / (Math.max(...chartData.revenues) * 1.2)) * 100}%"></div>
                    <div class="chart-bar expense" style="height: {(chartData.expenses[i] / (Math.max(...chartData.revenues) * 1.2)) * 100}%"></div>
                  </div>
                  <div class="chart-label">{label}</div>
                </div>
              {/each}
            {:else}
              {#each revenueData as data, i}
                <div class="chart-column">
                  <div class="chart-bar-container">
                    <div class="chart-bar revenue" style="height: {(data.amount / 60000) * 100}%"></div>
                    <div class="chart-bar expense" style="height: {(expenseData[i].amount / 60000) * 100}%"></div>
                  </div>
                  <div class="chart-label">{data.month}</div>
                </div>
              {/each}
            {/if}
          </div>
          <div class="chart-grid">
            <div class="grid-line">60K</div>
            <div class="grid-line">45K</div>
            <div class="grid-line">30K</div>
            <div class="grid-line">15K</div>
            <div class="grid-line">0</div>
          </div>
        </div>
      </div>
      
      <!-- Section des transactions récentes -->
      <div class="transactions-container">
        <div class="transactions-header">
          <h2>Transactions récentes</h2>
          <div class="search-container">
            <input 
              type="text" 
              bind:value={searchQuery} 
              placeholder="Rechercher..." 
              class="search-input"
            />
          </div>
        </div>
        
        <div class="transactions-table">
          <table>
            <thead>
              <tr>
                <th>Référence</th>
                <th>Client / Dépense</th>
                <th>Date</th>
                <th>Montant</th>
                <th>Statut</th>
              </tr>
            </thead>
            <tbody>
              {#each filteredTransactions as transaction}
                <tr>
                  <td><span class="transaction-id">{transaction.id}</span></td>
                  <td>{transaction.client}</td>
                  <td>{transaction.date}</td>
                  <td class={transaction.montant < 0 ? 'amount negative' : 'amount positive'}>
                    {transaction.montant.toLocaleString('fr-FR')} €
                  </td>
                  <td>
                    <span class="status-badge {transaction.statut}">
                      {transaction.statut}
                    </span>
                  </td>
                </tr>
              {/each}
              
              {#if filteredTransactions.length === 0}
                <tr>
                  <td colspan="5" class="no-results">Aucune transaction trouvée</td>
                </tr>
              {/if}
            </tbody>
          </table>
        </div>
        <div class="view-all">
          <a href="/transactions">Voir toutes les transactions</a>
        </div>
      </div>
    </div>
    
    <!-- Bottom Row -->
    <div class="dashboard-bottom">
      <!-- Factures à venir -->
      <div class="upcoming-container">
        <h2>Factures à payer prochainement</h2>
        <div class="upcoming-invoices">
          {#each upcomingInvoices as invoice, index}
            <div class="invoice-item">
              <div class="invoice-details">
                <h3>{invoice.fournisseur}</h3>
                <p>Échéance: {invoice.date}</p>
              </div>
              <div class="invoice-amount">
                {invoice.montant} €
              </div>
              <button class="pay-button" on:click={() => payInvoice(index)}>Payer</button>
            </div>
          {/each}
          
          {#if upcomingInvoices.length === 0}
            <div class="no-invoices">
              <p>Aucune facture à payer prochainement</p>
            </div>
          {/if}
        </div>
      </div>
      
      <!-- Actions rapides -->
      <div class="quick-actions">
        <h2>Actions rapides</h2>
        <div class="action-buttons">
          <button class="action-btn" on:click={toggleNewInvoiceModal}>
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
              <polyline points="14 2 14 8 20 8"></polyline>
              <line x1="16" y1="13" x2="8" y2="13"></line>
              <line x1="16" y1="17" x2="8" y2="17"></line>
              <polyline points="10 9 9 9 8 9"></polyline>
            </svg>
            Nouvelle facture
          </button>
          <button class="action-btn" on:click={toggleNewExpenseModal}>
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="12" y1="1" x2="12" y2="23"></line>
              <path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path>
            </svg>
            Enregistrer dépense
          </button>
          <button class="action-btn">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
              <line x1="16" y1="2" x2="16" y2="6"></line>
              <line x1="8" y1="2" x2="8" y2="6"></line>
              <line x1="3" y1="10" x2="21" y2="10"></line>
            </svg>
            Planifier paiement
          </button>
          <button class="action-btn" on:click={exportReport}>
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
              <polyline points="7 10 12 15 17 10"></polyline>
              <line x1="12" y1="15" x2="12" y2="3"></line>
            </svg>
            Exporter rapport
          </button>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Modals -->
  {#if showNewInvoiceModal}
    <div class="modal-overlay">
      <div class="modal">
        <div class="modal-header">
          <h2>Créer une nouvelle facture</h2>
          <button class="close-button" on:click={toggleNewInvoiceModal}>×</button>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label for="client">Client</label>
            <input type="text" id="client" bind:value={newInvoice.client} placeholder="Nom du client" class="form-control">
          </div>
          <div class="form-group">
            <label for="montant">Montant (€)</label>
            <input type="number" id="montant" bind:value={newInvoice.montant} placeholder="0.00" class="form-control">
          </div>
          <div class="form-group">
            <label for="date">Date</label>
            <input type="text" id="date" bind:value={newInvoice.date} placeholder="JJ/MM/AAAA" class="form-control">
          </div>
          <div class="form-actions">
            <button class="cancel-button" on:click={toggleNewInvoiceModal}>Annuler</button>
            <button class="submit-button" on:click={addNewInvoice}>Créer facture</button>
          </div>
        </div>
      </div>
    </div>
  {/if}
  
  {#if showNewExpenseModal}
    <div class="modal-overlay">
      <div class="modal">
        <div class="modal-header">
          <h2>Enregistrer une dépense</h2>
          <button class="close-button" on:click={toggleNewExpenseModal}>×</button>
        </div>
        <div class="modal-body">
          <div class="form-group">
            <label for="description">Description</label>
            <input type="text" id="description" bind:value={newExpense.description} placeholder="Description de la dépense" class="form-control">
          </div>
          <div class="form-group">
            <label for="expenseMontant">Montant (€)</label>
            <input type="number" id="expenseMontant" bind:value={newExpense.montant} placeholder="0.00" class="form-control">
          </div>
          <div class="form-group">
            <label for="expenseDate">Date</label>
            <input type="text" id="expenseDate" bind:value={newExpense.date} placeholder="JJ/MM/AAAA" class="form-control">
          </div>
          <div class="form-actions">
            <button class="cancel-button" on:click={toggleNewExpenseModal}>Annuler</button>
            <button class="submit-button" on:click={addNewExpense}>Enregistrer dépense</button>
          </div>
        </div>
      </div>
    </div>
  {/if}
  
  
  <style>
    .dashboard {
      padding: 1.5rem 0 0 15rem;
      background-color: #f5f7fa;
    }
    
    .dashboard-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      flex-wrap: wrap;
      gap: 1rem;
    }
    
    .dashboard-header h1 {
      font-size: 1.5rem;
      font-weight: 600;
      margin: 0;
      color: #1e293b;
    }
    
    .subtitle {
      color: #64748b;
      margin: 0.25rem 0 0 0;
      font-size: 0.9rem;
    }
    
    .date-filter {
      display: flex;
      align-items: center;
      gap: 1rem;
    }
    
    .period {
      font-weight: 500;
      color: #1e293b;
    }
    
    .filter-buttons {
      display: flex;
      background-color: #e2e8f0;
      border-radius: 0.5rem;
      padding: 0.25rem;
    }
    
    .filter-btn {
      background: none;
      border: none;
      padding: 0.5rem 1rem;
      font-size: 0.875rem;
      cursor: pointer;
      border-radius: 0.375rem;
      color: #64748b;
    }
    
    .filter-btn.active {
      background-color: #fff;
      color: #0f172a;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    }
    
    /* KPI Cards */
    .kpi-cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 1rem;
      margin-bottom: 1.5rem;
    }
    
    .kpi-card {
      background-color: #fff;
      border-radius: 0.75rem;
      padding: 1.25rem;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
      display: flex;
      align-items: flex-start;
      gap: 1rem;
    }
    
    .kpi-icon {
      background-color: #f1f5f9;
      width: 48px;
      height: 48px;
      border-radius: 0.5rem;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #3b82f6;
    }
    
    .kpi-content {
      flex: 1;
    }
    
    .kpi-title {
      color: #64748b;
      font-size: 0.875rem;
      margin: 0 0 0.25rem 0;
    }
    
    .kpi-value {
      font-size: 1.5rem;
      font-weight: 600;
      color: #0f172a;
      margin: 0;
    }
    
    .kpi-trend {
      display: flex;
      align-items: center;
      gap: 0.25rem;
      font-size: 0.875rem;
      margin: 0.25rem 0 0 0;
    }
    
    .kpi-trend.positive {
      color: #10b981;
    }
    
    .kpi-trend.negative {
      color: #ef4444;
    }
    
    /* Dashboard Content */
    .dashboard-content {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }
    
    .chart-container, .transactions-container {
      background-color: #fff;
      border-radius: 0.75rem;
      padding: 1.5rem;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    }
    
    .chart-container h2, .transactions-container h2, .upcoming-container h2, .quick-actions h2 {
      font-size: 1.125rem;
      font-weight: 600;
      margin: 0 0 1rem 0;
      color: #1e293b;
    }
    
    /* Chart Header and Legend */
    .chart-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      flex-wrap: wrap;
      gap: 1rem;
    }
    
    .chart-legend {
      display: flex;
      gap: 1rem;
    }
    
    .legend-item {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-size: 0.875rem;
      color: #64748b;
    }
    
    .legend-color {
      width: 12px;
      height: 12px;
      border-radius: 3px;
    }
    
    .legend-color.revenue {
      background-color: #3b82f6;
    }
    
    .legend-color.expense {
      background-color: #f59e0b;
    }
    
    /* Simplified Chart Visual */
    .chart-visual {
      height: 240px;
      position: relative;
      margin: 2rem 0 1rem;
    }
    
    .chart-columns {
      display: flex;
      justify-content: space-around;
      height: 100%;
      position: relative;
      z-index: 2;
    }
    
    .chart-column {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 14%;
    }
    
    .chart-bar-container {
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      gap: 4px;
    }
    
    .chart-bar {
      width: 45%;
      border-radius: 4px 4px 0 0;
      transition: height 0.3s ease;
    }
    
    .chart-bar.revenue {
      background-color: #3b82f6;
    }
    
    .chart-bar.expense {
      background-color: #f59e0b;
    }
    
    .chart-label {
      margin-top: 0.5rem;
      font-size: 0.75rem;
      color: #64748b;
    }
    
    .chart-grid {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 20px;  /* Space for labels */
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      z-index: 1;
    }
    
    .grid-line {
      position: relative;
      width: 100%;
      height: 1px;
      background-color: #e2e8f0;
      font-size: 0.65rem;
      color: #94a3b8;
      text-align: left;
      padding-left: 5px;
    }
    
    /* Transactions Table */
    .transactions-table {
      overflow-x: auto;
    }
    
    table {
      width: 100%;
      border-collapse: collapse;
    }
    
    th {
      text-align: left;
      padding: 0.75rem 1rem;
      font-size: 0.75rem;
      font-weight: 600;
      color: #64748b;
      border-bottom: 1px solid #e2e8f0;
    }
    
    td {
      padding: 0.75rem 1rem;
      font-size: 0.875rem;
      color: #1e293b;
      border-bottom: 1px solid #f1f5f9;
    }
    
    .transaction-id {
      font-family: monospace;
      font-size: 0.75rem;
      color: #64748b;
    }
    
    .amount {
      font-weight: 500;
    }
    
    .amount.positive {
      color: #10b981;
    }
    
    .amount.negative {
      color: #ef4444;
    }
    
    .status-badge {
      display: inline-block;
      padding: 0.25rem 0.75rem;
      border-radius: 9999px;
      font-size: 0.75rem;
      font-weight: 500;
      text-transform: capitalize;
    }
    
    .status-badge.payée {
      background-color: #dcfce7;
      color: #15803d;
    }
    
    .status-badge.en {
      background-color: #fef3c7;
      color: #b45309;
    }
    
    .status-badge.débitée {
      background-color: #f1f5f9;
      color: #475569;
    }
    
    .view-all {
      margin-top: 1rem;
      text-align: center;
    }
    
    .view-all a {
      color: #3b82f6;
      text-decoration: none;
      font-size: 0.875rem;
      font-weight: 500;
    }
    
    /* Bottom Row */
    .dashboard-bottom {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 1.5rem;
    }
    
    .upcoming-container, .quick-actions {
      background-color: #fff;
      border-radius: 0.75rem;
      padding: 1.5rem;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    }
    
    /* Upcoming Invoices */
    .upcoming-invoices {
      display: flex;
      flex-direction: column;
      gap: 0.75rem;
    }
    
    .invoice-item {
      display: flex;
      align-items: center;
      padding: 0.75rem;
      border-radius: 0.5rem;
      background-color: #f8fafc;
      gap: 1rem;
    }
    
    .invoice-details {
      flex: 1;
    }
    
    .invoice-details h3 {
      margin: 0;
      font-size: 0.875rem;
      font-weight: 500;
      color: #0f172a;
    }
    
    .invoice-details p {
      margin: 0.25rem 0 0 0;
      font-size: 0.75rem;
      color: #64748b;
    }
    
    .invoice-amount {
      font-weight: 600;
      color: #1e293b;
      margin-right: 1rem;
    }
    
    .pay-button {
      background-color: #3b82f6;
      color: white;
      border: none;
      border-radius: 0.375rem;
      padding: 0.375rem 0.75rem;
      font-size: 0.75rem;
      font-weight: 500;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    
    .pay-button:hover {
      background-color: #2563eb;
    }
    
    /* Quick Actions */
    .action-buttons {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: 0.75rem;
    }
    
    .action-btn {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 0.75rem;
      padding: 1rem;
      background-color: #f8fafc;
      border: none;
      border-radius: 0.5rem;
      color: #1e293b;
      font-size: 0.75rem;
      font-weight: 500;
      cursor: pointer;
      transition: background-color 0.3s, transform 0.2s;
      text-align: center;
    }
    
    .action-btn svg {
      color: #3b82f6;
    }
    
    .action-btn:hover {
      background-color: #f1f5f9;
      transform: translateY(-2px);
    }
    
    /* Responsive Adjustments */
    @media (max-width: 768px) {
      .dashboard-content {
        grid-template-columns: 1fr;
      }
      
      .kpi-cards {
        grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      }
    }
    
    @media (max-width: 576px) {
      .dashboard-header {
        flex-direction: column;
        align-items: flex-start;
      }
      
      .date-filter {
        width: 100%;
        justify-content: space-between;
      }
      
      .kpi-cards {
        grid-template-columns: 1fr;
      }
      
      .action-buttons {
        grid-template-columns: repeat(2, 1fr);
      }
    }
  </style>