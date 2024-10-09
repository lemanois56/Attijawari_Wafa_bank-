<template>
  <PageHeader :propData="dataToPass" />

  <!-- Start::row-1 - Formulaire de Virement -->
  <div class="row">
    <div class="col-xl-8 mx-auto">
      <div class="card custom-card shadow-lg">
        <div class="card-header">
          <div class="card-title">
            Nouveau Virement
          </div>
        </div>
        <div class="card-body">
          <!-- Formulaire de virement -->
          <form @submit.prevent="submitTransfer">
            <div class="mb-3">
              <label for="beneficiary" class="form-label">Bénéficiaire</label>
              <input
                  type="text"
                  class="form-control"
                  id="beneficiary"
                  v-model="transfer.beneficiary"
                  placeholder="Nom du bénéficiaire"
                  required
              />
            </div>
            <div class="mb-3">
              <label for="iban" class="form-label">IBAN du Bénéficiaire</label>
              <input
                  type="text"
                  class="form-control"
                  id="iban"
                  v-model="transfer.iban"
                  placeholder="Numéro IBAN"
                  required
              />
            </div>
            <div class="mb-3">
              <label for="amount" class="form-label">Montant (MAD)</label>
              <input
                  type="number"
                  class="form-control"
                  id="amount"
                  v-model="transfer.amount"
                  placeholder="Montant"
                  required
              />
            </div>
            <div class="mb-3">
              <label for="executionDate" class="form-label">Date d'exécution</label>
              <input
                  type="date"
                  class="form-control"
                  id="executionDate"
                  v-model="transfer.executionDate"
                  required
              />
            </div>
            <div class="mb-3">
              <label for="reason" class="form-label">Motif</label>
              <textarea
                  class="form-control"
                  id="reason"
                  v-model="transfer.reason"
                  placeholder="Motif du virement"
                  rows="3"
                  required
              ></textarea>
            </div>
            <div class="mb-3">
              <label for="recurring" class="form-label">Virement récurrent</label>
              <select
                  class="form-select"
                  id="recurring"
                  v-model="transfer.recurring"
              >
                <option value="non">Non</option>
                <option value="hebdomadaire">Hebdomadaire</option>
                <option value="mensuel">Mensuel</option>
                <option value="annuel">Annuel</option>
              </select>
            </div>
            <div class="mb-3 form-check">
              <input
                  type="checkbox"
                  class="form-check-input"
                  id="confirmDetails"
                  v-model="transfer.confirmDetails"
              />
              <label class="form-check-label" for="confirmDetails">
                J'ai vérifié les informations saisies
              </label>
            </div>
            <button
                type="submit"
                class="btn btn-primary"
                :disabled="!transfer.confirmDetails"
            >
              Valider le Virement
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
  <!-- End::row-1 -->

  <!-- Start::row-2 - Résumé du Virement -->
  <div class="row mt-4" v-if="showSummary">
    <div class="col-xl-8 mx-auto">
      <div class="card custom-card shadow-lg">
        <div class="card-header">
          <div class="card-title">Résumé du Virement</div>
        </div>
        <div class="card-body">
          <p><strong>Bénéficiaire :</strong> {{ transfer.beneficiary }}</p>
          <p><strong>IBAN :</strong> {{ transfer.iban }}</p>
          <p><strong>Montant :</strong> {{ transfer.amount.toLocaleString('fr-FR', { style: 'currency', currency: 'EUR' }) }}</p>
          <p><strong>Date d'exécution :</strong> {{ transfer.executionDate }}</p>
          <p><strong>Motif :</strong> {{ transfer.reason }}</p>
          <p><strong>Virement récurrent :</strong> {{ transfer.recurring !== 'non' ? transfer.recurring : 'Non' }}</p>
          <button class="btn btn-success" @click="confirmTransfer">
            Confirmer le Virement
          </button>
          <button class="btn btn-secondary ms-2" @click="cancelTransfer">
            Annuler
          </button>
        </div>
      </div>
    </div>
  </div>
  <!-- End::row-2 -->
</template>

<script lang="ts">
import PageHeader from "@/components/common/pageheader.vue";
import auth from '@/middleware/auth';
import { ref } from 'vue';

export default {
  setup() {
    definePageMeta({
      middleware: [auth],
    });

    // Données du virement
    const transfer = ref({
      beneficiary: '',
      iban: '',
      amount: 0,
      executionDate: '',
      reason: '',
      recurring: 'non',
      confirmDetails: false,
    });

    // Afficher ou masquer le résumé du virement
    const showSummary = ref(false);

    // Fonction de soumission du formulaire de virement
    const submitTransfer = () => {
      // Vérifier que toutes les informations requises sont remplies
      if (transfer.value.beneficiary && transfer.value.iban && transfer.value.amount && transfer.value.executionDate && transfer.value.reason) {
        showSummary.value = true;
      }
    };

    // Fonction de confirmation du virement
    const confirmTransfer = () => {
      alert('Virement confirmé !');
      // Réinitialiser les données après confirmation
      transfer.value = {
        beneficiary: '',
        iban: '',
        amount: 0,
        executionDate: '',
        reason: '',
        recurring: 'non',
        confirmDetails: false,
      };
      showSummary.value = false;
    };

    // Fonction d'annulation du virement
    const cancelTransfer = () => {
      showSummary.value = false;
    };

    // Données du composant PageHeader
    const dataToPass = {
      current: "Nouveau Virement",
      list: ['Virements', 'Nouveau Virement']
    };

    return {
      dataToPass,
      transfer,
      showSummary,
      submitTransfer,
      confirmTransfer,
      cancelTransfer
    };
  },
  components: {
    PageHeader
  },
};
</script>

<style scoped>
.card {
  border-radius: 12px;
}

.card-header {
  background: #007bff;
  color: #fff;
}

.card-title {
  font-size: 1.2rem;
  font-weight: 600;
}

.form-check-input {
  cursor: pointer;
}

.btn {
  min-width: 120px;
}

.btn-success {
  background-color: #28a745;
  border-color: #28a745;
}

.btn-secondary {
  background-color: #6c757d;
  border-color: #6c757d;
}
</style>
