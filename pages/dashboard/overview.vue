<script setup lang="ts">
import * as crmData from '@/data/dashboards/crm.js';
import { DoughnutChart } from 'vue-chart-3';
import { definePageMeta } from '#imports';
import { ref } from 'vue';
import ApexCharts from 'vue3-apexcharts';
import auth from '@/middleware/auth';

definePageMeta({
  layout: 'maindashboard',
  middleware: [auth],
});

// Données bancaires et de progression
const accountBalance = ref("41 086 278"); // Solde du compte
const targetProgress = ref(48); // Progression en %

const creditCardBalance = ref(-2500.00); // Exemple de solde carte de crédit
const creditCardLimit = ref(5000.00); // Limite carte de crédit

const savingsBalance = ref(8000.00); // Solde épargne
const savingsGrowth = ref(5.2); // Croissance épargne en pourcentage

const recentTransactions = ref([
  { date: '2023-10-05', description: 'Supermarché Carrefour', amount: -150.30, type: 'Débit' },
  { date: '2023-10-04', description: 'Virement reçu', amount: 1200.00, type: 'Crédit' },
  { date: '2023-10-03', description: 'Paiement en ligne Amazon', amount: -89.99, type: 'Débit' },
  { date: '2023-10-01', description: 'Salaire', amount: 2500.00, type: 'Crédit' },
  // Ajoutez plus de transactions si nécessaire
]);

const spendingData = {
  labels: ['Alimentation', 'Transport', 'Loisirs', 'Factures', 'Autres'],
  datasets: [{
    data: [500, 200, 300, 400, 150],
    backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF']
  }]
};

// Options pour le cercle de progression
const progressOptions = ref({
  chart: {
    type: 'radialBar',
    height: 120,
    width: 120,
    sparkline: {
      enabled: true
    }
  },
  plotOptions: {
    radialBar: {
      startAngle: -90,
      endAngle: 90,
      hollow: {
        margin: 0,
        size: "60%",
        background: "transparent"
      },
      track: {
        background: "#fff",
        strokeWidth: '97%',
        margin: 5,
        dropShadow: {
          enabled: false,
        }
      },
      dataLabels: {
        show: true,
        value: {
          formatter: function (val: number) {
            return val + "%";
          },
          color: "#fff",
          fontSize: "16px",
          show: true,
        }
      }
    }
  },
  fill: {
    type: 'solid',
    colors: ['#ffffff']
  },
  stroke: {
    lineCap: 'round'
  },
  labels: ['Progress'],
  colors: ['#fff'],
});

const progressSeries = ref([targetProgress.value]); // La progression en %

const balanceOptions = {
  chart: {
    type: 'line',
    toolbar: {
      show: false
    },
    height: 200,
  },
  stroke: {
    curve: 'smooth'
  },
  xaxis: {
    categories: ['Jan', 'Fév', 'Mar', 'Avr', 'Mai', 'Juin', 'Juil', 'Août', 'Sep', 'Oct']
  },
  colors: ['#4BC0C0'],
};

const balanceSeries = [{
  name: 'Solde',
  data: [10000, 12000, 9000, 11000, 13000, 12500, 14000, 13500, 15000, 12500]
}];

const incomeExpenseOptions = {
  chart: {
    type: 'bar',
    stacked: true,
    toolbar: {
      show: false
    },
    height: 200,
  },
  xaxis: {
    categories: ['Jan', 'Fév', 'Mar', 'Avr', 'Mai', 'Juin', 'Juil', 'Août', 'Sep', 'Oct']
  },
  colors: ['#28a745', '#dc3545'],
};

const incomeExpenseSeries = [{
  name: 'Revenus',
  data: [2500, 2600, 2400, 2550, 2700, 2650, 2800, 2750, 2900, 2850]
}, {
  name: 'Dépenses',
  data: [2000, 1800, 2100, 1900, 2200, 2000, 2300, 2150, 2400, 2250]
}];

const financialGoals = ref([
  { goal: 'Épargner pour les vacances', progress: 60 },
  { goal: 'Achat d\'une nouvelle voiture', progress: 35 },
  { goal: 'Constitution d\'un fonds d\'urgence', progress: 80 },
]);

const latestNews = ref([
  { title: 'Mise à jour des taux d\'intérêt', date: '2023-10-05' },
  { title: 'Nouveaux services bancaires mobiles', date: '2023-10-03' },
  { title: 'Conseils pour économiser', date: '2023-10-01' },
]);

const recentActivities = ref([
  {
    icon: 'bi bi-circle-fill fs-8',
    iconBg: 'bg-primary-transparent',
    description: 'Connexion depuis un nouvel appareil',
    time: 'Il y a 2 heures'
  },
  {
    icon: 'bi bi-circle-fill fs-8',
    iconBg: 'bg-success-transparent',
    description: 'Mise à jour de votre profil',
    time: 'Hier'
  },
  {
    icon: 'bi bi-circle-fill fs-8',
    iconBg: 'bg-warning-transparent',
    description: 'Alerte de sécurité',
    time: 'Il y a 3 jours'
  },
]);
</script>

<template>
  <!-- Début du tableau de bord -->
  <div class="row">
    <!-- Nouvelle carte rouge avec progression -->
    <div class="col-lg-3 col-md-6">
      <div class="card custom-card progress-card">
        <div class="card-body">
          <div class="d-flex align-items-center justify-content-between">
            <div>
              <div class="fw-semibold fs-16 text-white mb-1">Solde Disponible</div>
              <h4 class="fw-bold text-white mb-1">{{ accountBalance }} MAD</h4>
              <a href="javascript:void(0);" class="text-white">Voir plus</a>
            </div>
<!--            <div>-->
<!--              <apexchart type="radialBar" :options="progressOptions" :series="progressSeries"></apexchart>-->
<!--            </div>-->
          </div>
        </div>
      </div>
    </div>
    <!-- Autres cartes d'information réduites -->
    <div class="col-lg-3 col-md-6">
      <div class="card custom-card info-card">
        <div class="card-body">
          <p class="text-muted mb-0">Carte de Crédit</p>
          <h5 class="fw-semibold mt-1">{{ creditCardBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'MAD' }) }}</h5>
          <p class="mb-0 text-danger fw-semibold">Limite : {{ creditCardBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'MAD' }) }}</p>
        </div>
      </div>
    </div>
    <div class="col-lg-3 col-md-6">
      <div class="card custom-card info-card">
        <div class="card-body">
          <p class="text-muted mb-0">Compte Épargne</p>
          <h5 class="fw-semibold mt-1">{{ savingsBalance.toLocaleString('fr-FR', { style: 'currency', currency: 'MAD' }) }}</h5>
          <p class="mb-0 text-success fw-semibold">+{{ savingsGrowth }}% ce mois</p>
        </div>
      </div>
    </div>
    <div class="col-lg-3 col-md-6">
      <div class="card custom-card info-card">
        <div class="card-body">
          <p class="text-muted mb-0">Objectifs Financiers</p>
          <h5 class="fw-semibold mt-1">{{ financialGoals.length }}</h5>
        </div>
      </div>
    </div>
    <!-- Graphiques réduits -->
    <div class="col-xl-6">
      <div class="card custom-card">
        <div class="card-header">
          <div class="card-title">
            Évolution du Solde
          </div>
        </div>
        <div class="card-body">
          <apexchart type="line" :options="balanceOptions" :series="balanceSeries"></apexchart>
        </div>
      </div>
    </div>
    <div class="col-xl-6">
      <div class="card custom-card">
        <div class="card-header">
          <div class="card-title">
            Revenus vs Dépenses
          </div>
        </div>
        <div class="card-body">
          <apexchart type="bar" :options="incomeExpenseOptions" :series="incomeExpenseSeries"></apexchart>
        </div>
      </div>
    </div>
    <!-- Transactions récentes réduites et responsives -->
    <div class="col-xl-6 col-lg-12">
      <div class="card custom-card">
        <div class="card-header d-flex justify-content-between align-items-center">
          <div class="card-title">
            Transactions Récentes
          </div>
          <button class="btn btn-sm btn-primary d-lg-none" @click="toggleDetails">
            {{ showDetails ? 'Masquer' : 'Voir Plus' }}
          </button>
        </div>
        <div class="card-body p-0 table-responsive">
          <table class="table text-nowrap table-hover mb-0">
            <thead class="table-light">
            <tr>
              <th>Date</th>
              <th>Description</th>
              <th>Montant</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(transaction, index) in recentTransactions.slice(0, showDetails ? 5 : 3)" :key="index">
              <td>{{ transaction.date }}</td>
              <td>{{ transaction.description }}</td>
              <td :class="{'text-success': transaction.type === 'Crédit', 'text-danger': transaction.type === 'Débit'}">
                {{ transaction.amount.toLocaleString('fr-FR', { style: 'currency', currency: 'EUR' }) }}
              </td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- Objectifs financiers -->
    <div class="col-xl-6">
      <div class="card custom-card">
        <div class="card-header">
          <div class="card-title">
            Objectifs Financiers
          </div>
        </div>
        <div class="card-body">
          <ul class="list-group">
            <li v-for="(goal, index) in financialGoals" :key="index" class="list-group-item">
              <div class="d-flex justify-content-between">
                <span>{{ goal.goal }}</span>
                <span>{{ goal.progress }}%</span>
              </div>
              <div class="progress progress-xs mt-2">
                <div class="progress-bar" :style="{ width: goal.progress + '%' }"></div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <!-- Actualités -->
    <div class="col-xl-4">
      <div class="card custom-card">
        <div class="card-header">
          <div class="card-title">
            Actualités Bancaires
          </div>
        </div>
        <div class="card-body">
          <ul class="list-unstyled">
            <li v-for="(news, index) in latestNews" :key="index" class="mb-3">
              <h6 class="mb-1">{{ news.title }}</h6>
              <small class="text-muted">{{ news.date }}</small>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Graphique des dépenses par catégorie réduit -->
    <div class="col-xl-4">
      <div class="card custom-card">
        <div class="card-header">
          <div class="card-title">
            Dépenses par Catégorie
          </div>
        </div>
        <div class="card-body">
          <DoughnutChart :chartData="spendingData" :options="{ responsive: true, maintainAspectRatio: false }" />
        </div>
      </div>
    </div>

    <!-- Activités récentes réduites -->
    <div class="col-xl-4">
      <div class="card custom-card">
        <div class="card-header">
          <div class="card-title">
            Activités Récentes
          </div>
        </div>
        <div class="card-body">
          <ul class="list-unstyled mb-0">
            <li v-for="(activity, index) in recentActivities" :key="index" class="mb-3">
              <div class="d-flex align-items-top">
                <div class="me-2">
                    <span :class="['avatar', 'avatar-xs', 'avatar-rounded', activity.iconBg]">
                      <i :class="activity.icon"></i>
                    </span>
                </div>
                <div>
                  <span>{{ activity.description }}</span>
                  <div class="text-muted fs-12">{{ activity.time }}</div>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div> <!-- Fin de la row principale -->
  <!-- Section de droite -->
  <div class="col-xxl-3 col-xl-12">
    <div class="row">


      <!-- Offre spéciale réduite -->
      <div class="col-xl-12">
        <div class="card custom-card">
          <img src="https://mabanque.bnpparibas/content/dam/mabanque/rsc/contrib/html/particuliers/home/images/virements_internationaux.jpg" class="card-img-top" alt="Offre spéciale">
          <div class="card-body text-center">
            <h6 class="card-title">Profitez de notre nouvelle offre !</h6>
            <p class="card-text">Obtenez un prêt à taux réduit jusqu'au 31 décembre.</p>
            <a href="javascript:void(0);" class="btn btn-primary btn-sm">En savoir plus</a>
          </div>
        </div>
      </div>
    </div>
  </div> <!-- Fin de la col-xxl-3 -->

  <!-- Fin du tableau de bord -->
</template>

<style scoped>
/* Style pour la carte rouge personnalisée */
.progress-card {
  background-color: #196d30; /* Couleur rouge */
  border-radius: 10px;
  color: #fff;
  padding: 15px;
}

.progress-card .card-body {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.text-white {
  color: #ffffff !important;
}

/* Styles pour réduire la taille des éléments */
.info-card {
  padding: 15px;
}
.info-card h5 {
  font-size: 1.2rem;
}
.card.custom-card {
  margin-bottom: 15px;
}
.card.custom-card .card-body {
  padding: 15px;
}
.table th, .table td {
  padding: 8px;
  font-size: 0.9rem;
}
.list-group-item {
  padding: 10px 15px;
}
.list-group-item .progress {
  height: 6px;
}
.avatar-xs {
  width: 30px;
  height: 30px;
}
.card-img-top {
  height: 100px;
  object-fit: cover;
}
</style>

