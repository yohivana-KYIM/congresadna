<template>
    <Separator />
    <br />
    <div class="min-h-screen flex items-center justify-center bg-gradient-to-br from-[#9C7C13] to-[#D4AF37] p-6">
      <div class="bg-white shadow-2xl rounded-2xl p-8 w-full max-w-2xl">
        <h2 class="text-4xl font-bold text-center mb-8 text-[#9C7C13]">Enregistrement du Sponsor</h2>
  
        <div class="mb-8 relative pt-1">
          <div class="flex mb-2 items-center justify-between">
            <div>
              <span class="text-xs font-semibold inline-block py-1 px-2 uppercase rounded-full text-[#9C7C13] bg-[#D4AF37] bg-opacity-20">
                Progression
              </span>
            </div>
            <div class="text-right">
              <span class="text-xs font-semibold inline-block text-[#9C7C13]">
                {{ Math.round((step - 1) * (100 / 2)) }}%
              </span>
            </div>
          </div>
          <div class="overflow-hidden h-2 mb-4 text-xs flex rounded bg-[#D4AF37] bg-opacity-20">
            <div 
              class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-[#9C7C13]"
              :style="{ width: (step - 1) * (100 / 2) + '%' }"
            ></div>
          </div>
        </div>
  
        <div v-if="step === 1" class="transition-opacity duration-500 ease-in-out">
          <h3 class="text-2xl font-semibold mb-6 text-[#9C7C13]">Étape 1 : Informations Personnelles</h3>
          <form @submit.prevent="nextStep" class="space-y-6">
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Nom complet</label>
              <input v-model="sponsorData.name" type="text" placeholder="Nom complet" class="input-field" />
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Email</label>
              <input v-model="sponsorData.email" type="email" placeholder="Email" class="input-field" />
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Mot de passe</label>
              <input v-model="sponsorData.password" type="password" placeholder="Mot de passe" class="input-field" />
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Confirmer le mot de passe</label>
              <input v-model="sponsorData.password_confirmation" type="password" placeholder="Confirmer le mot de passe" class="input-field" />
            </div>
            <div class="flex justify-end">
              <button type="submit" class="btn-next">Suivant</button>
            </div>
          </form>
        </div>
  
        <div v-if="step === 2" class="transition-opacity duration-500 ease-in-out">
          <h3 class="text-2xl font-semibold mb-6 text-[#9C7C13]">Étape 2 : Informations de l'Entreprise</h3>
          <form @submit.prevent="nextStep" class="space-y-6">
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Nom de l'entreprise</label>
              <input v-model="sponsorData.nomEntreprise" type="text" placeholder="Nom de l'entreprise" class="input-field" />
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Nom du dirigeant</label>
              <input v-model="sponsorData.nomDirigeant" type="text" placeholder="Nom du dirigeant" class="input-field" />
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Email du dirigeant</label>
              <input v-model="sponsorData.emailDirigeant" type="email" placeholder="Email du dirigeant" class="input-field" />
            </div>
            <div class="flex justify-between space-x-4">
              <button type="button" @click="prevStep" class="btn-prev">Précédent</button>
              <button type="submit" class="btn-next">Suivant</button>
            </div>
          </form>
        </div>
  
        <div v-if="step === 3" class="transition-opacity duration-500 ease-in-out">
          <h3 class="text-2xl font-semibold mb-6 text-[#9C7C13]">Étape 3 : Informations Complémentaires</h3>
          <form @submit.prevent="registerSponsor" class="space-y-6">
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Comment contacter</label>
              <textarea v-model="sponsorData.commentContacter" placeholder="Comment contacter" class="input-field h-24 resize-none"></textarea>
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Site web</label>
              <input v-model="sponsorData.siteWeb" type="url" placeholder="Site web" class="input-field" />
            </div>
            <div class="space-y-2">
              <label class="block text-gray-700 font-medium">Niveau de sponsoring</label>
              <select v-model="sponsorData.niveauSponsoring" class="input-field">
                <option value="" disabled>Sélectionnez un niveau</option>
                <option value="bronze">Bronze</option>
                <option value="argent">Argent</option>
                <option value="or">Or</option>
              </select>
            </div>
            <div class="flex justify-between space-x-4">
              <button type="button" @click="prevStep" class="btn-prev">Précédent</button>
              <button type="submit" class="btn-submit">S'enregistrer</button>
            </div>
          </form>
        </div>
  
        <div v-if="errorMessage" class="mt-6 p-4 bg-red-100 border-l-4 border-red-500 text-red-700">
          <p class="font-bold">Erreur</p>
          <p>{{ errorMessage }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { useAuthStore } from "@/stores/auth";
  import { useToast } from 'vue-toastification';
  import Separator from "@/components/frontend/separator/Separator.vue";
  const toast = useToast();
  
  export default {
    components: {
  
    Separator,
  },
    data() {
      return {
        step: 1,
        sponsorData: {
          name: "",
          email: "",
          password: "",
          password_confirmation: "",
          nomEntreprise: "",
          nomDirigeant: "",
          emailDirigeant: "",
          commentContacter: "",
          siteWeb: "",
          niveauSponsoring: "",
        },
        errorMessage: null,
      };
    },
    methods: {
      nextStep() {
        if (this.step < 3) {
          this.step++;
        }
      },
      prevStep() {
        if (this.step > 1) {
          this.step--;
        }
      },
      async registerSponsor() {
        const authStore = useAuthStore();
        try {
          await authStore.registerSponsor(this.sponsorData);
          toast.success('Inscription réussie sponsor!');
          this.$router.push({ name: "home" });
        } catch (error) {
          console.error('Erreur de l\'API :', error.response.data);
          this.errorMessage = error.response?.data?.message || "Une erreur s'est produite";
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .input-field {
    @apply w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#9C7C13] focus:border-transparent transition duration-200 ease-in-out;
  }
  
  .btn-next, .btn-prev, .btn-submit {
    @apply px-6 py-3 rounded-lg text-white font-semibold transition duration-200 ease-in-out transform hover:-translate-y-1 hover:shadow-lg;
  }
  
  .btn-next {
    @apply bg-[#9C7C13] hover:bg-[#7a6210];
  }
  
  .btn-prev {
    @apply bg-[#458018] hover:bg-[#3d7a15];
  }
  
  .btn-submit {
    @apply bg-[#9C7C13] hover:bg-[#7a6210];
  }
  </style>