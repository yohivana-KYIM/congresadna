<template>
  <Separator />
  <br />
  <div
    class="min-h-screen bg-gradient-to-r from-[#E8F5E9] to-[#FFF3E0] flex items-center justify-center p-6 relative z-0"
  >
    <div
      class="w-full max-w-4xl bg-white rounded-3xl shadow-2xl overflow-hidden relative z-10 transform hover:scale-[1.01] transition-transform duration-300"
    >
      <!-- Barre de progression -->
      <div class="w-full px-6 pt-6">
        <div class="flex justify-between mb-4">
          <div
            v-for="step in totalSteps"
            :key="step"
            class="flex flex-col items-center"
          >
            <div
              class="flex items-center justify-center w-8 h-8 rounded-full"
              :class="{
                'bg-[#06AE16] text-white': currentStep >= step,
                'bg-gray-200': currentStep < step,
              }"
            >
              {{ step }}
            </div>
            <div
              class="mt-1 text-sm"
              :class="{
                'text-[#06AE16] font-semibold': currentStep >= step,
                'text-gray-400': currentStep < step,
              }"
            >
              Étape {{ step }}
            </div>
          </div>
        </div>
        <div class="w-full h-2 bg-gray-200 rounded-full">
          <div
            class="bg-[#06AE16] h-2 rounded-full transition-all duration-500"
            :style="{
              width: `${((currentStep - 1) / (totalSteps - 1)) * 100}%`,
            }"
          ></div>
        </div>
      </div>

      <div class="flex flex-col md:flex-row">
        <!-- Section de gauche -->
        <div
          class="relative w-full p-8 overflow-hidden bg-white md:w-1/2 rounded-l-3xl"
        >
          <div class="relative z-20">
            <h1
              class="mb-4 text-4xl font-bold text-gray-800 transition-all duration-500"
            >
              <span class="typed-text">{{ displayText }}</span>
              <span class="animate-blink">|</span>
            </h1>
            <p class="mb-6 text-gray-600 animate-fade-in">
              Nous sommes ravis de vous accueillir ! Inscrivez-vous dès
              maintenant pour participer à cet événement exceptionnel et
              rejoignez-nous pour partager des idées et des expériences
              enrichissantes.
            </p>
          </div>
          <!-- Bulles décoratives -->
          <div
            class="absolute top-0 right-0 -translate-y-1/4 translate-x-1/4 w-64 h-64 rounded-full bg-[#06AE16] opacity-20 animate-pulse"
          ></div>
          <div
            class="absolute bottom-0 left-0 translate-y-1/4 -translate-x-1/4 w-64 h-64 rounded-full bg-[#FDA304] opacity-20 animate-pulse"
          ></div>
        </div>

        <!-- Section droite - Formulaire par étapes -->
        <div
          class="relative z-20 w-full p-6 lg:w-1/2 md:p-10 bg-gray-50 rounded-r-3xl"
        >
          <form @submit.prevent="handleSubmit" class="space-y-8">
            <!-- Étape 1: Informations de base -->
            <div v-if="currentStep === 1" class="space-y-4">
              <div class="form-group">
                <label>Nom complet</label>
                <input
                  v-model="formData.name"
                  type="text"
                  class="form-input"
                  required
                />
                <span v-if="errors.name" class="text-sm text-red-500">{{
                  errors.name
                }}</span>
              </div>
              <div class="form-group">
                <label>Email</label>
                <input
                  v-model="formData.email"
                  type="email"
                  class="form-input"
                  required
                />
                <span v-if="errors.email" class="text-sm text-red-500">{{
                  errors.email
                }}</span>
              </div>

              <div class="relative form-group">
                <label>Mot de passe</label>
                <input
                  v-model="formData.password"
                  :type="showPassword ? ' text' : 'password'"
                  class="pr-10 form-input"
                  required
                />
                <button
                  type="button"
                  @click="togglePassword('password')"
                  class="absolute inset-y-0 right-0 flex items-center pr-3 text-sm leading-5"
                >
                  <EyeIcon v-if="!showPassword" class="w-5 h-5 text-gray-500" />
                  <EyeSlashIcon v-else class="w-5 h-5 text-gray-500" />
                </button>
                <span v-if="errors.password" class="text-sm text-red-500">{{
                  errors.password[0]
                }}</span>
              </div>

              <!-- Champ de confirmation du mot de passe -->
              <div class="relative form-group">
                <label>Confirmer le mot de passe</label>
                <input
                  v-model="formData.password_confirmation"
                  :type="showPasswordConfirmation ? 'text' : 'password'"
                  class="pr-10 form-input"
                  required
                />
                <button
                  type="button"
                  @click="togglePassword('confirmation')"
                  class="absolute inset-y-0 right-0 flex items-center pr-3 text-sm leading-5"
                >
                  <EyeIcon
                    v-if="!showPasswordConfirmation"
                    class="w-5 h-5 text-gray-500"
                  />
                  <EyeSlashIcon v-else class="w-5 h-5 text-gray-500" />
                </button>
                <span
                  v-if="errors.password_confirmation"
                  class="text-sm text-red-500"
                  >{{ errors.password_confirmation[0] }}</span
                >
              </div>
            </div>
            <!-- Étape 2: Détails de contact -->
            <div v-if="currentStep === 2" class="space-y-4">
              <div class="form-group">
                <label>Adresse</label>
                <input
                  v-model="formData.adresse"
                  type="text"
                  class="form-input"
                  required
                />
                <span v-if="errors.adresse" class="text-sm text-red-500">{{
                  errors.adresse
                }}</span>
              </div>
              <div class="form-group">
                <label>Numéro de téléphone</label>
                <input
                  v-model="formData.numeroTelephone"
                  type="tel"
                  class="form-input"
                  required
                />
                <span
                  v-if="errors.numeroTelephone"
                  class="text-sm text-red-500"
                  >{{ errors.numeroTelephone }}</span
                >
              </div>
              <div class="form-group">
                <label>État civil</label>
                <select
                  v-model="formData.etatCivil"
                  class="form-input"
                  required
                >
                  <option value="">Sélectionnez</option>
                  <option value="celibataire">Célibataire</option>
                  <option value="marie">Marié(e)</option>
                  <option value="divorce">Divorcé(e)</option>
                  <option value="veuf">Veuf/Veuve</option>
                </select>
                <span v-if="errors.etatCivil" class="text-sm text-red-500">{{
                  errors.etatCivil
                }}</span>
              </div>
              <div class="form-group">
                <label>Pays</label>
                <select
                  v-model="selectedPaysId"
                  @change="handlePaysChange"
                  class="form-input"
                  required
                >
                  <option value="">Sélectionnez un pays</option>
                  <option
                    v-for="pays in paysList"
                    :key="pays.id"
                    :value="pays.id"
                  >
                    {{ pays.nom }}
                  </option>
                </select>
                <span v-if="errors.pays" class="text-sm text-red-500">{{
                  errors.pays
                }}</span>
              </div>
            </div>

            <!-- Étape 3: Localisation -->
            <div v-if="currentStep === 3" class="space-y-4">
              <div class="form-group">
                <label>Ville</label>
                <select
                  v-model="selectedVilleId"
                  class="form-input"
                  required
                  :disabled="!selectedPaysId"
                >
                  <option value="">Sélectionnez une ville</option>
                  <option
                    v-for="ville in villesList"
                    :key="ville.id"
                    :value="ville.id"
                  >
                    {{ ville.nom }}
                  </option>
                </select>
                <span v-if="errors.ville" class="text-sm text-red-500">{{
                  errors.ville
                }}</span>
              </div>
              <div class="form-group">
                <label>Canton NDOGBATJECK</label>
                <select
                  v-model="formData.estCantonNonParlant"
                  @change="handleCantonChange"
                  class="form-input"
                  required
                >
                  <option value="">Sélectionnez</option>
                  <option value="oui">Oui</option>
                  <option value="non">Non</option>
                </select>
                <span
                  v-if="errors.estCantonNonParlant"
                  class="text-sm text-red-500"
                  >{{ errors.estCantonNonParlant }}</span
                >
              </div>
              <div
                v-if="formData.estCantonNonParlant === 'oui'"
                class="form-group"
              >
                <label>Village</label>
                <select
                  v-model="formData.village_id"
                  class="form-input"
                  required
                >
                  <option value="">Sélectionnez un village</option>
                  <option
                    v-for="village in villageList"
                    :key="village.id"
                    :value="village.id"
                  >
                    {{ village.nom }}
                  </option>
                </select>
                <span v-if="errors.village_id" class="text-sm text-red-500">{{
                  errors.village_id[0]
                }}</span>
              </div>

              <div class="form-group">
                <label class="block mb-1 text-sm font-medium text-gray-700">
                  Souhaitez-vous participer à une activité au congrès ?
                </label>
                <div class="space-y-2">
                  <div
                    v-for="activite in activitesList"
                    :key="activite.id"
                    class="flex items-center"
                  >
                    <input
                      type="checkbox"
                      :id="`activite-${activite.id}`"
                      v-model="selectedActivites"
                      :value="activite.id"
                      class="mr-2"
                    />
                    <label :for="`activite-${activite.id}`" class="text-sm">
                      {{ activite.NomActivite }} ({{ activite.DateDebut }} -
                      {{ activite.Lieu }})
                    </label>
                  </div>
                </div>
              </div>
            </div>

            <!-- Étape 4: Activités et Thèmes -->
            <div v-if="currentStep === 4" class="space-y-4">
              <div class="form-group">
                <label>Êtes-vous conférencier ?</label>
                <select
                  v-model="formData.estConferencier"
                  @change="handleConferencierChange"
                  class="form-input"
                  required
                >
                  <option value="">Sélectionnez</option>
                  <option value="oui">Oui</option>
                  <option value="non">Non</option>
                </select>
              </div>
              <!-- Themes Selection -->
              <div v-if="showThemes" class="form-group">
                <label class="block mb-1 text-sm font-medium text-gray-700">
                  Veuillez sélectionner un ou plusieurs thèmes
                </label>
                <div class="space-y-2">
                  <div
                    v-for="theme in themeList"
                    :key="theme.id"
                    class="flex items-center"
                  >
                    <input
                      type="checkbox"
                      :id="`theme-${theme.id}`"
                      v-model="formData.themes"
                      :value="theme.id"
                      class="w-4 h-4 text-[#06AE16] border-gray-300 rounded focus:ring-[#06AE16]"
                    />
                    <label
                      :for="`theme-${theme.id}`"
                      class="ml-2 text-gray-700"
                      >{{ theme.nom }}</label
                    >
                  </div>
                </div>
              </div>

              <div class="form-group">
                <label>Est vous Membre de l'association</label>
                <select
                  v-model="formData.estMembreAssociation"
                  class="form-input"
                  required
                >
                  <option value="">Sélectionnez</option>
                  <option value="oui">Oui</option>
                  <option value="non">Non</option>
                </select>
                <span
                  v-if="errors.estMembreAssociation"
                  class="text-sm text-red-500"
                  >{{ errors.estMembreAssociation }}</span
                >
              </div>
            </div>

            <!-- Boutons de navigation -->
            <div class="flex flex-wrap justify-between gap-4 mt-8">
              <button
                type="button"
                v-if="currentStep > 1"
                @click="prevStep"
                class="flex items-center gap-2 px-6 py-3 font-medium text-gray-700 transition-all duration-300 bg-gray-200 hover:bg-gray-300 rounded-xl"
              >
                <span>←</span> Retour
              </button>

              <button
                type="button"
                v-if="currentStep < totalSteps"
                @click="nextStep"
                class="px-6 py-3 bg-[#06AE16] hover:bg-[#058512] text-white rounded-xl font-medium transition-all duration-300 flex items-center gap-2"
              >
                Suivant <span>→</span>
              </button>

              <button
                v-if="
                  currentStep === totalSteps &&
                  formData.estMembreAssociation === 'oui'
                "
                type="button"
                @click="handlePayment"
                :disabled="isLoading"
                class="px-6 py-3 bg-[#06AE16] hover:bg-[#058512] text-white rounded-xl font-medium transition-all duration-300 flex items-center gap-2"
              >
                <span v-if="isLoading">Chargement...</span>
                <span v-else>Passer aux paiements</span>
              </button>
              <button
                v-if="
                  currentStep === totalSteps &&
                  formData.estMembreAssociation !== 'oui'
                "
                type="submit"
                class="px-6 py-3 bg-[#06AE16] hover:bg-[#058512] text-white rounded-xl font-medium transition-all duration-300 flex items-center gap-2"
              >
                Terminer ✓
              </button>

              <button
                type="button"
                @click="saveForLater"
                class="flex items-center gap-2 px-6 py-3 font-medium text-white transition-all duration-300 bg-amber-500 hover:bg-amber-600 rounded-xl"
              >
                <span>💾</span> Sauvegarder
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import PaysService from "@/services/PaysService";
import VilleService from "@/services/VilleService";
import VillageService from "@/services/VillageService";
import ThemeService from "@/services/ThemeService";
import ActiviteService from "@/services/ActiviteService";
import { useAuthStore } from "@/stores/auth";
import Swal from "sweetalert2";
import { nanoid } from "nanoid";
import Separator from "@/components/frontend/separator/Separator.vue";
import { useRouter } from "vue-router";
import { EyeIcon, EyeSlashIcon } from "@heroicons/vue/24/solid"; // Pour les icônes pleines
export default {
  name: "Inscription",
  components: {
    EyeIcon,

    EyeSlashIcon,
    Separator,
  },
  setup() {
    const router = useRouter();
    const authStore = useAuthStore();
    const paysList = ref([]);
    const villesList = ref([]);
    const villageList = ref([]);
    const themeList = ref([]);
    const activitesList = ref([]);
    const isLoading = ref(false);
    const errors = ref({});
    const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    // const phoneRegex = /^\+((33[0-9]{9})|(237[0-9]{8}))$/;
    // // Format: +33XXXXXXXXX (10 chiffres après +33) ou +237XXXXXXXX (8 chiffres après +237)
    // const phoneRegex = /^\+237[0-9]{9}$/;
    const phoneRegex = /^\+[1-9]\d{1,3}[0-9]{6,12}$/;

    const showPassword = ref(false);

    const showPasswordConfirmation = ref(false);

    const togglePassword = (field) => {
      if (field === "password") {
        showPassword.value = !showPassword.value;
      } else if (field === "confirmation") {
        showPasswordConfirmation.value = !showPasswordConfirmation.value;
      }
    };
    const displayText = ref("");
    const typedIndex = ref(0);

    const typedStrings = ref([
      "Inscrivez-vous maintenant au Congrès NDOGBATJECK!",
      "Rejoignez-nous pour une expérience inoubliable!",
      "Venez célébrer et partager nos valeurs!",
      "Participez au Congrès et faites entendre votre voix!",
      "Soyez acteur du changement, rejoignez-nous!",
      "Unissons nos forces au Congrès NDOGBATJECK!",
    ]);
    const selectedPaysId = ref("");
    const selectedVilleId = ref("");
    const showVillages = ref(false);
    const showThemes = ref(false);
    const selectedActivites = ref([]);
    const formData = ref({
      name: "",
      email: "",
      password: "",
      password_confirmation: "",
      adresse: "",
      numeroTelephone: "",
      etatCivil: "",
      pays_id: "",
      ville_id: "",
      estMembreAssociation: false,
      estCantonNonParlant: false,
      estConferencier: false,
      village_id: null,
      activite_id: [],
      themes: [],
      type_paiement: "",
    });
    const currentStep = ref(1);
    const totalSteps = 4;

    onMounted(async () => {
      try {
        const response = await PaysService.getAllPays();
        paysList.value = response.data?.data || response.data || [];
        startTypedText();
        await getActivites();
      } catch (error) {
        console.error("Erreur lors du chargement des pays:", error);
      }
    });

    const startTypedText = () => {
      setInterval(() => {
        typedIndex.value = (typedIndex.value + 1) % typedStrings.value.length;
        displayText.value = typedStrings.value[typedIndex.value];
      }, 3000);
    };

    const handlePaysChange = async () => {
      selectedVilleId.value = "";
      villesList.value = [];

      if (selectedPaysId.value) {
        try {
          isLoading.value = true;
          const response = await VilleService.getVillesByPays(
            selectedPaysId.value
          );
          villesList.value = Array.isArray(response)
            ? response
            : response.data || [];
          formData.value.pays_id = selectedPaysId.value;
        } catch (error) {
          console.error("Erreur lors du chargement des villes:", error);
        } finally {
          isLoading.value = false;
        }
      }
    };

    const fetchVillages = async () => {
      try {
        villageList.value = await VillageService.getAllVillages();
      } catch (error) {
        console.error("Erreur lors de la récupération des villages:", error);
      }
    };

    const handleCantonChange = () => {
      if (formData.value.estCantonNonParlant === "oui") {
        fetchVillages();
      } else {
        formData.value.village_id = null;
      }
    };

    const getActivites = async () => {
      try {
        const activities = await ActiviteService.getAllActivites();
        activitesList.value = Array.isArray(activities) ? activities : [];
      } catch (error) {
        console.error("Erreur lors de la récupération des activités:", error);
      }
    };

    const handleConferencierChange = () => {
      showThemes.value = formData.value.estConferencier === "oui";
      if (showThemes.value) {
        getThemes();
      } else {
        formData.value.themes = [];
      }
    };

    const getThemes = async () => {
      try {
        themeList.value = await ThemeService.getAllThemes();
      } catch (error) {
        console.error("Erreur lors de la récupération des thèmes:", error);
      }
    };

    const validateCurrentStep = () => {
      errors.value = {}; // Réinitialiser les erreurs
      let isValid = true;

      // Validation pour l'étape 1
      if (currentStep.value === 1) {
        if (!formData.value.name) {
          errors.value.name = "Le nom est requis";
          isValid = false;
        }
        if (!formData.value.email) {
          errors.value.email = "L'email est requis";
          isValid = false;
        } else if (!emailRegex.test(formData.value.email)) {
          errors.value.email =
            "L'email doit être au format valide (ex: exemple@gmail.com)";
          isValid = false;
        }
        if (!formData.value.password) {
          errors.value.password = "Le mot de passe est requis";
          isValid = false;
        }
        if (!formData.value.password_confirmation) {
          errors.value.password_confirmation =
            "La confirmation du mot de passe est requise";
          isValid = false;
        }
      }

      // Validation pour l'étape 2
      if (currentStep.value === 2) {
        if (!formData.value.adresse) {
          errors.value.adresse = "L'adresse est requise";
          isValid = false;
        }
        if (!formData.value.numeroTelephone) {
          errors.value.numeroTelephone = "Le numéro de téléphone est requis";
          isValid = false;
        } else if (!phoneRegex.test(formData.value.numeroTelephone)) {
          errors.value.numeroTelephone =
            "Le numéro de téléphone doit être au format  +237XXXXXXXX";
          isValid = false;
        }
        if (!formData.value.etatCivil) {
          errors.value.etatCivil = "L'état civil est requis";
          isValid = false;
        }
        if (!formData.value.pays_id) {
          errors.value.pays = "Le pays est requis";
          isValid = false;
        }
      }

      if (!isValid) {
        Swal.fire({
          title: "Erreur",
          text: "Veuillez remplir tous les champs obligatoires.",
          icon: "warning",
          confirmButtonText: "OK",
        });
      }

      return isValid;
    };
    const handleSubmit = async () => {
      if (!validateCurrentStep()) return;

      let dataToSubmit = { ...formData.value };
      dataToSubmit.ville_id = selectedVilleId.value;
      dataToSubmit.estMembreAssociation =
        dataToSubmit.estMembreAssociation === "oui";
      dataToSubmit.estCantonNonParlant =
        dataToSubmit.estCantonNonParlant === "oui";
      dataToSubmit.estConferencier = dataToSubmit.estConferencier === "oui";
      dataToSubmit.activite_id = selectedActivites.value;

      if (!dataToSubmit.estCantonNonParlant) {
        delete dataToSubmit.village_id;
      }

      if (dataToSubmit.estConferencier) {
        dataToSubmit.themes = formData.value.themes;
      } else {
        delete dataToSubmit.themes;
      }

      isLoading.value = true;
      try {
        await authStore.registerParticipant(dataToSubmit);
        Swal.fire({
          title: "Succès!",
          text: "inscription réussie! ",
          icon: "success",
          confirmButtonText: "OK",
        }).then(() => {
          //router.push('/login');
        });
        console.log("Pré-inscription réussie !");
      } catch (error) {
        console.error("Erreur lors de la  Pré-inscription:", error);
        if (error.response && error.response.data.errors) {
          errors.value = error.response.data.errors;
        }
        Swal.fire("Erreur", errorMessage, "error");
      } finally {
        isLoading.value = false;
      }
    };

    const goToPayment = () => {
      // Logique pour rediriger vers la page de paiement
      router.push("/paiement"); // Remplacez '/paiement' par le chemin de votre page de paiement
    };

    //pour save avant le paiement

    const handlePayment = async () => {
      if (!validateCurrentStep()) return;

      isLoading.value = true; // Commencer le chargement

      let dataToSubmit = { ...formData.value };
      dataToSubmit.ville_id = selectedVilleId.value;
      dataToSubmit.estMembreAssociation =
        dataToSubmit.estMembreAssociation === "oui";
      dataToSubmit.estCantonNonParlant =
        dataToSubmit.estCantonNonParlant === "oui";
      dataToSubmit.estConferencier = dataToSubmit.estConferencier === "oui";
      dataToSubmit.activite_id = selectedActivites.value;

      if (!dataToSubmit.estCantonNonParlant) {
        delete dataToSubmit.village_id;
      }

      if (dataToSubmit.estConferencier) {
        dataToSubmit.themes = formData.value.themes;
      } else {
        delete dataToSubmit.themes;
      }

      try {
        await authStore.registerParticipant(dataToSubmit);
        goToPayment(); // Rediriger vers la page de paiement après l'inscription
      } catch (error) {
        console.error("Erreur lors de la pré-inscription:", error);
        if (error.response && error.response.data.errors) {
          errors.value = error.response.data.errors;
        }
      } finally {
        isLoading.value = false; // Arrêter le chargement
      }
    };

    const saveForLater = async () => {
      try {
        // Générer un ID unique pour la sauvegarde
        const saveId = nanoid(10);

        // Créer l'objet de sauvegarde
        const saveData = {
          formData: formData.value,
          currentStep: currentStep.value,
          timestamp: new Date().toISOString(),
        };

        // Sauvegarder dans le localStorage
        localStorage.setItem(
          `registration_${saveId}`,
          JSON.stringify(saveData)
        );

        // Générer le lien de reprise
        const resumeLink = `${window.location.origin}/inscription?saveId=${saveId}`;

        // Afficher le lien à l'utilisateur
        await Swal.fire({
          title: "Sauvegarde réussie!",
          html: `
            <p>Voici votre lien pour reprendre l'inscription plus tard:</p>
            <div class="p-2 mt-2 bg-gray-100 rounded select-all">
              <code>${resumeLink}</code>
            </div>
            <p class="mt-2 text-sm">Copiez et conservez ce lien précieusement!</p>
          `,
          icon: "success",
          confirmButtonText: "Copier le lien",
          showCancelButton: true,
          cancelButtonText: "Fermer",
        }).then((result) => {
          if (result.isConfirmed) {
            navigator.clipboard.writeText(resumeLink);
            Swal.fire(
              "Copié!",
              "Le lien a été copié dans votre presse-papiers",
              "success"
            );
          }
        });
      } catch (error) {
        console.error("Erreur lors de la sauvegarde:", error);
        Swal.fire(
          "Erreur",
          "Impossible de sauvegarder votre progression",
          "error"
        );
      }
    };

    const loadSavedData = (saveId) => {
      try {
        const savedData = localStorage.getItem(`registration_${saveId}`);
        if (savedData) {
          const { formData: savedFormData, currentStep: savedCurrentStep } =
            JSON.parse(savedData);
          formData.value = savedFormData;
          currentStep.value = savedCurrentStep;
          return true;
        }
        return false;
      } catch (error) {
        console.error("Erreur lors du chargement de la sauvegarde:", error);
        return false;
      }
    };

    onMounted(() => {
      fetchVillages();
      getActivites();
      handlePaysChange;
      handleCantonChange;
      // Vérifier si on a un ID de sauvegarde dans l'URL
      const urlParams = new URLSearchParams(window.location.search);
      const saveId = urlParams.get("saveId");

      if (saveId) {
        const loaded = loadSavedData(saveId);
        if (!loaded) {
          Swal.fire(
            "Erreur",
            "Impossible de charger la sauvegarde demandée",
            "error"
          );
        }
      }
    });

    const prevStep = () => {
      if (currentStep.value > 1) {
        currentStep.value--;
      }
    };

    const nextStep = () => {
      if (validateCurrentStep()) {
        if (currentStep.value < totalSteps) {
          currentStep.value++;
        }
      }
    };

    return {
      formData,
      selectedPaysId,
      selectedVilleId,
      paysList,
      villesList,
      villageList,
      themeList,
      activitesList,
      isLoading,
      errors,
      showPassword,

      showPasswordConfirmation,

      togglePassword,
      displayText,
      handleSubmit,
      handlePaysChange,
      handleCantonChange,
      showVillages,
      showThemes,
      handleConferencierChange,
      selectedActivites,
      currentStep,
      totalSteps,
      prevStep,
      nextStep,
      saveForLater,
      goToPayment,
      handlePayment,
    };
  },
};
</script>

<style scoped>
.form-input {
  @apply w-full px-4 py-3 border border-gray-300 rounded-xl focus:ring-2 

       focus:ring-[#06AE16] focus:border-transparent transition-all duration-300;
}

.form-group {
  @apply space-y-2;
}

.form-group label {
  @apply block text-sm font-medium text-gray-700;
}

.btn-primary {
  @apply bg-[#06AE16] hover:bg-[#058512] text-white px-6 py-3 rounded-xl

       font-medium transition-all duration-300 transform hover:scale-105

       focus:outline-none focus:ring-2 focus:ring-[#06AE16] focus:ring-offset-2;
}

.btn-secondary {
  @apply bg-gray-200 hover:bg-gray-300 text-gray-700 px-6 py-3 rounded-xl

       font-medium transition-all duration-300 transform hover:scale-105

       focus:outline-none focus:ring-2 focus:ring-gray-400 focus:ring-offset-2;
}

.loader {
  border: 2px solid white;
  border-top: 2px solid #06ae16;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.animate-blink {
  animation: blink-animation 1s steps(5, start) infinite;
}

@keyframes blink-animation {
  from,
  to {
    border-color: transparent;
  }
  50% {
    border-color: #06ae16;
  }
}

.animate-fade-in {
  animation: fade-in 0.5s ease-in-out forwards;
}

@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.animate-slide-up {
  animation: slide-up 0.5s ease-in-out forwards;
}

@keyframes slide-up {
  from {
    transform: translateY(10px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.bulle {
  position: absolute;
  top: 0;
  right: 0;
  transform: translate(-25%, -25%);
  width: 64px;
  height: 64px;
  border-radius: 50%;
  background-color: #06ae16;
  opacity: 0.2;
  animation: pulse 2s infinite;
}

.bulle:nth-child(2) {
  transform: translate(-75%, 25%);
  background-color: #fda304;
}

@keyframes pulse {
  0% {
    transform: scale(0.9);
  }
  100% {
    transform: scale(1);
  }
}
</style>
