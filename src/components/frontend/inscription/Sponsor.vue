<template>
    <div class="min-h-screen bg-gradient-to-br from-[#9C7C13] to-[#D4AF37] flex justify-center items-center py-12 px-4 sm:px-6 lg:px-8 relative overflow-hidden">
      <!-- Improved Bubble Effects -->
      <div v-for="n in 20" :key="n" 
           :class="`bubble bubble-${n}`"
           :style="{
             left: `${Math.random() * 100}%`,
             top: `${Math.random() * 100}%`,
             '--size': `${20 + Math.random() * 60}px`,
             '--delay': `${Math.random() * 4}s`
           }">
      </div>
  
      <!-- Book Container -->
      <div class="book-container relative w-full max-w-7xl h-[600px] perspective-1000">
        <div class="w-full h-full transition-transform duration-1000 book transform-style-preserve-3d" :class="{ 'rotate-y-180': isBookOpen }">
          <!-- Front Cover (Welcome) -->
          <div class="absolute flex flex-col items-center justify-center w-full h-full p-8 text-center bg-white rounded-lg shadow-2xl backface-hidden">
            <h1 class="text-4xl font-bold text-[#9C7C13] mb-6">Bienvenue au Congrès</h1>
            <p class="mb-8 text-xl text-gray-600">Rejoignez-nous pour un événement exceptionnel</p>
            <button @click="openBook" class="px-6 py-3 bg-[#9C7C13] text-white rounded-md shadow-lg hover:bg-[#D4AF37] transition-colors duration-300">
              S'inscrire maintenant
            </button>
            <button @click="openLogin" class="px-4 py-2 mt-4 text-gray-700 bg-gray-200 rounded-md hover:bg-gray-300">Connexion</button>
          </div>
  
          <!-- Back Cover (Registration Form) -->
          <div class="absolute w-full h-full p-8 overflow-y-auto bg-white rounded-lg shadow-2xl backface-hidden rotate-y-180">
            <h2 class="text-3xl font-bold text-[#9C7C13] text-center mb-6">Inscription au Congrès</h2>
            
            <!-- Progress Bar -->
            <div class="mb-8">
              <div class="relative pt-1">
                <div class="flex items-center justify-between mb-2">
                  <div>
                    <span class="text-xs font-semibold inline-block py-1 px-2 uppercase rounded-full text-[#9C7C13] bg-yellow-200">
                      Progression
                    </span>
                  </div>
                  <div class="text-right">
                    <span class="text-xs font-semibold inline-block text-[#9C7C13]">
                      {{ Math.round((currentStep / 3) * 100) }}%
                    </span>
                  </div>
                </div>
                <div class="flex h-2 mb-4 overflow-hidden text-xs bg-yellow-200 rounded">
                  <div :style="{ width: `${(currentStep / 3) * 100}%` }" class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-[#9C7C13] transition-all duration-500 ease-in-out"></div>
                </div>
              </div>
            </div>
  
            <form @submit.prevent="handleSubmit" class="space-y-6">
              <!-- Step 1: Personal Information -->
              <TransitionGroup name="fade" mode="out-in">
                <div v-if="currentStep === 1" key="step1" class="space-y-4">
                  <h3 class="text-lg font-semibold text-center text-gray-900">Informations de contact</h3>
                  <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
                    <div>
                      <label for="name" class="block text-sm font-medium text-gray-700">Nom complet</label>
                      <input id="name" v-model="form.name" type="text" required
                            class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out"/>
                    </div>
                    <div>
                      <label for="email" class="block text-sm font-medium text-gray-700">Adresse e-mail</label>
                      <input id="email" v-model="form.email" type="email" required
                            class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out"/>
                    </div>
                    <div>
                      <label for="numeroTelephone" class="block text-sm font-medium text-gray-700">Numéro de téléphone</label>
                      <input id="numeroTelephone" v-model="form.numeroTelephone" type="text" required
                            class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out"/>
                    </div>
                  </div>
                </div>
  
                <!-- Step 2: Additional Information -->
                <div v-if="currentStep === 2" key="step2" class="space-y-4">
                  <h3 class="text-lg font-semibold text-center text-gray-900">Informations supplémentaires</h3>
                  <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
                    <div>
                      <label for="adresse" class="block text-sm font-medium text-gray-700">Adresse</label>
                      <input id="adresse" v-model="form.adresse" type="text" required
                            class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out"/>
                    </div>
                    <div>
                      <label for="ID_Pays" class="block text-sm font-medium text-gray-700">Pays</label>
                      <!-- <select id="ID_Pays" v-model="form.ID_Pays" required class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out">
                        <option value="">Sélectionnez un pays</option>
                        <option v-for="pays in paysList" :key="pays.id" :value="pays.id">{{ pays.nom }}</option>
                      </select> -->
  
  
  
                      <select id="ID_Pays" v-model="form.ID_Pays" required class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out">
    <option value="">Sélectionnez un pays</option>
    <option v-for="pay in paysList" :key="pay.id" :value="pay.id">{{ pay.nom }}</option>
  </select>
  
                    </div>
                    <div>
                      <label for="ID_Ville" class="block text-sm font-medium text-gray-700">Ville</label>
                      <select id="ID_Ville" v-model="form.ID_Ville" required class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out">
                        <option value="">Sélectionnez une ville</option>
                        <option v-for="ville in villesList" :key="ville.id" :value="ville.id">{{ ville.nom }}</option>
                      </select>
                    </div>
                    <div>
                      <label for="password" class="block text-sm font-medium text-gray-700">Mot de passe</label>
                      <input id="password" v-model="form.password" type="password" required
                            class="mt-1 block w-full px-3 py-2 bg-white border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-[#9C7C13] focus:border-[#9C7C13] transition duration-150 ease-in-out"/>
                    </div>
                  </div>
                  <div class="flex flex-col space-y-2">
                    <label class="inline-flex items-center">
                      <input type="checkbox" v-model="form.estMembreAssociation" class="form-checkbox text-[#9C7C13]">
                      <span class="ml-2">Membre d'association</span>
                    </label>
                    <label class="inline-flex items-center">
                      <input type="checkbox" v-model="form.estCantonNonParlant" class="form-checkbox text-[#9C7C13]">
                      <span class="ml-2">Canton non parlant</span>
                    </label>
                  </div>
                </div>
  
                <!-- Step 3: Confirmation -->
                <div v-if="currentStep === 3" key="step3" class="space-y-4">
                  <h3 class="text-lg font-semibold text-center text-gray-900">Confirmation</h3>
                  <p class="text-sm text-center text-gray-600">Veuillez confirmer vos informations :</p>
                  <div class="flex flex-col space-y-2">
                    <div class="flex justify-between">
                      <span>Nom complet:</span>
                      <span>{{ form.name }}</span>
                    </div>
                    <div class="flex justify-between">
                      <span>Adresse e-mail:</span>
                      <span>{{ form.email }}</span>
                    </div>
                    <div class="flex justify-between">
                      <span>Numéro de téléphone:</span>
                      <span>{{ form.numeroTelephone }}</span>
                    </div>
                    <div class="flex justify-between">
                      <span>Adresse:</span>
                      <span>{{ form.adresse }}</span>
                    </div>
                    <div class="flex justify-between">
                      <span>Pays:</span>
                      <span>{{ paysList.find(pays => pays.id === form.ID_Pays)?.nom }}</span>
                    </div>
                    <div class="flex justify-between">
                      <span>Ville:</span>
                      <span>{{ villesList.find(ville => ville.id === form.ID_Ville)?.nom }}</span>
                    </div>
                  </div>
                </div>
              </TransitionGroup>
  
              <div class="flex justify-between">
                <button v-if="currentStep > 1" @click="prevStep" class="px-4 py-2 text-gray-700 bg-gray-200 rounded-md hover:bg-gray-300">Retour</button>
                <button v-if="currentStep < 3" @click="nextStep" class="px-4 py-2 bg-[#9C7C13] text-white rounded-md shadow-lg hover:bg-[#D4AF37] transition-colors duration-300">Suivant</button>
                <button v-if="currentStep === 3" type="submit" class="px-4 py-2 bg-[#9C7C13] text-white rounded-md shadow-lg hover:bg-[#D4AF37] transition-colors duration-300">Soumettre</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  
  
  <script>
  import { useAuthStore } from '@/stores/auth';  // Utilisez le bon chemin pour l'auth store
  import PaysService from '@/services/paysService';     // Utilisez le bon chemin pour pays service
  import VillesService from '@/services/VilleServiceervice'; // Utilisez le bon chemin pour villes service
  
  export default {
    data() {
      return {
        currentStep: 1,
        form: {
          name: '',
          email: '',
          numeroTelephone: '',
          adresse: '',
          ID_Pays: '',
          ID_Ville: '',
          password: '',
          estMembreAssociation: false,
          estCantonNonParlant: false
        },
        paysList: [],
        villesList: [],
        isBookOpen: false,
      };
    },
    async created() {
      await this.fetchPays();
      await this.fetchVilles();
    },
    methods: {
      async fetchPays() {
        try {
          this.paysList = await PaysService.getAllPays();
          console.log('Pays List:', this.paysList);
        } catch (error) {
          console.error('Erreur lors de la récupération des pays:', error);
        }
      },
     
      async fetchVilles() {
        try {
          const response = await VillesService.getAllVilles();
          if (response && response.length > 0) {
            this.villesList = response;
            console.log('Villes List:', this.villesList);
          } else {
            console.error('No villes found or response is empty.');
            this.villesList = [];
          }
        } catch (error) {
          console.error('Erreur lors de la récupération des villes:', error);
          this.villesList = [];
        }
      },
  
      openBook() {
        this.isBookOpen = true;
        this.currentStep = 1;
      },
      openLogin() {
        // Logique pour ouvrir login
      },
      nextStep() {
        if (this.currentStep < 3) {
          this.currentStep++;
        }
      },
      prevStep() {
        if (this.currentStep > 1) {
          this.currentStep--;
        }
      },
      async handleSubmit() {
        const authStore = useAuthStore();
        try {
          await authStore.registerParticipant(this.form);
          // Logique après une inscription réussie (par exemple, rediriger vers login ou dashboard)
          console.log('Inscription réussie:', this.form);
        } catch (error) {
          console.error('Erreur lors de l\'inscription:', error);
        }
      },
      resetForm() {
        this.form = {
          name: '',
          email: '',
          numeroTelephone: '',
          adresse: '',
          ID_Pays: '',
          ID_Ville: '',
          password: '',
          estMembreAssociation: false,
          estCantonNonParlant: false
        };
        this.currentStep = 1;
        this.isBookOpen = false;
      }
    },
  };
  </script>
  
  
  
  <style scoped>
  .bubble {
    position: absolute;
    border-radius: 50%;
    opacity: 0.6;
    animation: rise 8s infinite;
  }
  
  .bubble-1 {
    background-color: rgba(255, 255, 255, 0.8);
  }
  
  .bubble-2 {
    background-color: rgba(255, 255, 255, 0.7);
  }
  
  .bubble-3 {
    background-color: rgba(255, 255, 255, 0.6);
  }
  
  .bubble-4 {
    background-color: rgba(255, 255, 255, 0.5);
  }
  
  .bubble-5 {
    background-color: rgba(255, 255, 255, 0.4);
  }
  
  .bubble-6 {
    background-color: rgba(255, 255, 255, 0.3);
  }
  
  .bubble-7 {
    background-color: rgba(255, 255, 255, 0.2);
  }
  
  .bubble-8 {
    background-color: rgba(255, 255, 255, 0.1);
  }
  
  @keyframes rise {
    0% {
      transform: translateY(0);
    }
    100% {
      transform: translateY(-100%);
    }
  }
  
  /* Book Styles */
  .book-container {
    perspective: 1500px;
  }
  
  .book {
    position: relative;
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
  }
  
  .backface-hidden {
    backface-visibility: hidden;
  }
  
  .rotate-y-180 {
    transform: rotateY(180deg);
  }
  
  /* Fade Transition */
  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  </style>
  