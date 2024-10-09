<template>
  <PageHeader :propData="dataToPass" />

  <!-- Start::row-1 - Vue d'ensemble des comptes -->
  <div class="row">
    <!-- Compte Courant -->
    <div class="col-lg-3 col-md-6 mb-4">
      <div class="card custom-card info-card shadow-lg">
        <div class="card-body d-flex align-items-center">
          <div class="icon-container me-3">
            <i class="bi bi-wallet2"></i>
          </div>
          <div>
            <p class="text-muted mb-1">Compte Courant</p>
            <h5 class="fw-semibold mb-1">{{ checkingAccountBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'EUR' }) }}</h5>
            <a href="javascript:void(0);" class="text-primary">Voir détails</a>
          </div>
        </div>
      </div>
    </div>

    <!-- Compte Épargne -->
    <div class="col-lg-3 col-md-6 mb-4">
      <div class="card custom-card info-card shadow-lg">
        <div class="card-body d-flex align-items-center">
          <div class="icon-container me-3">
            <i class="bi bi-piggy-bank"></i>
          </div>
          <div>
            <p class="text-muted mb-1">Compte Épargne</p>
            <h5 class="fw-semibold mb-1">{{ savingsAccountBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'EUR' }) }}</h5>
            <a href="javascript:void(0);" class="text-primary">Voir détails</a>
          </div>
        </div>
      </div>
    </div>

    <!-- Carte de Crédit -->
    <div class="col-lg-3 col-md-6 mb-4">
      <div class="card custom-card info-card shadow-lg">
        <div class="card-body d-flex align-items-center">
          <div class="icon-container me-3">
            <i class="bi bi-credit-card"></i>
          </div>
          <div>
            <p class="text-muted mb-1">Carte de Crédit</p>
            <h5 class="fw-semibold mb-1">{{ creditCardBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'EUR' }) }}</h5>
            <a href="javascript:void(0);" class="text-primary">Voir détails</a>
          </div>
        </div>
      </div>
    </div>

    <!-- Prêts -->
    <div class="col-lg-3 col-md-6 mb-4">
      <div class="card custom-card info-card shadow-lg">
        <div class="card-body d-flex align-items-center">
          <div class="icon-container me-3">
            <i class="bi bi-building"></i>
          </div>
          <div>
            <p class="text-muted mb-1">Prêts</p>
            <h5 class="fw-semibold mb-1">{{ loanBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'EUR' }) }}</h5>
            <a href="javascript:void(0);" class="text-primary">Voir détails</a>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!--End::row-1 -->

  <!-- Start::row-2 - Historique et autres détails -->
  <div class="row">
    <!-- Historique des Transactions -->
    <div class="col-lg-6 col-md-12 mb-4">
      <div class="card custom-card shadow-lg">
        <div class="card-header">
          <div class="card-title">
            Historique des Transactions
          </div>
        </div>
        <div class="card-body p-0">
          <table class="table text-nowrap table-hover mb-0">
            <thead>
            <tr>
              <th>Date</th>
              <th>Description</th>
              <th>Montant</th>
            </tr>
            </thead>
            <tbody>
            <!-- Exemples de transactions -->
            <tr>
              <td>27/09/2024</td>
              <td>Achat Supermarché</td>
              <td class="text-danger">-450,00 MAD</td>
            </tr>
            <tr>
              <td>26/09/2024</td>
              <td>Virement Salaire</td>
              <td class="text-success">+12 000,00 MAD</td>
            </tr>
            <tr>
              <td>25/09/2024</td>
              <td>Paiement Facture Internet</td>
              <td class="text-danger">-400,00 MAD</td>
            </tr>
            <tr>
              <td>24/09/2024</td>
              <td>Retrait DAB</td>
              <td class="text-danger">-2 000,00 MAD</td>
            </tr>
            <tr>
              <td>23/09/2024</td>
              <td>Transfert de fonds</td>
              <td class="text-danger">-3 000,00 MAD</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- Graphe des Dépenses -->
    <div class="col-lg-6 col-md-12 mb-4">
      <div class="card custom-card shadow-lg">
        <div class="card-header">
          <div class="card-title">
            Graphe des Dépenses
          </div>
        </div>
        <div class="card-body">
          <apexchart type="bar" :options="expensesOptions" :series="expensesSeries"></apexchart>
        </div>
      </div>
    </div>
  </div>
  <!--End::row-2 -->
</template>

<script lang="ts">
import PageHeader from "@/components/common/pageheader.vue";
import auth from '@/middleware/auth';
import { ref } from 'vue';
import ApexCharts from 'vue3-apexcharts';

export default {
  setup() {
    definePageMeta({
      middleware: [auth],
    });

    // Données pour les différents comptes
    const checkingAccountBalance = ref(5); // Exemple de solde compte courant
    const savingsAccountBalance = ref(1210000); // Exemple de solde compte épargne
    const creditCardBalance = ref(-2500); // Exemple de solde carte de crédit
    const loanBalance = ref(100000); // Exemple de solde de prêt

    // Données pour le graphique des dépenses
    const expensesSeries = ref([
      {
        name: "Dépenses",
        data: [500, 700, 800, 1000, 1200, 1500, 1300]
      }
    ]);
    const expensesOptions = ref({
      chart: {
        height: 350,
        type: 'bar'
      },
      plotOptions: {
        bar: {
          borderRadius: 4,
          horizontal: false,
        }
      },
      dataLabels: {
        enabled: false
      },
      xaxis: {
        categories: ['Lun', 'Mar', 'Mer', 'Jeu', 'Ven', 'Sam', 'Dim'],
      }
    });

    const dataToPass = {
      current: "Vue d'ensemble",
      list: ['Comptes', 'Vue d\'ensemble']
    };

    return {
      dataToPass,
      checkingAccountBalance,
      savingsAccountBalance,
      creditCardBalance,
      loanBalance,
      expensesSeries,
      expensesOptions
    };
  },
  components: {
    PageHeader,
    ApexCharts
  },
};
</script>

<style scoped>
/* Styles communs pour les cartes d'information */
.info-card {
  border-radius: 12px;
  padding: 10px;
}

.card-body {
  padding: 20px;
}

.icon-container {
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  background-color: #f8f9fa;
  border-radius: 50%;
}

.shadow-lg {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.text-primary {
  color: #007bff !important;
}

/* Styles supplémentaires */
.table th, .table td {
  padding: 10px;
  font-size: 0.9rem;
}
</style>
