<template>
  <div class="h-screen flex">
    <TransitionRoot as="template" :show="sidebarOpen">
      <Dialog as="div" class="fixed inset-0 flex z-40 lg:hidden" @close="sidebarOpen = false">
        <TransitionChild as="template" enter="transition-opacity ease-linear duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="transition-opacity ease-linear duration-300" leave-from="opacity-100" leave-to="opacity-0">
          <DialogOverlay class="fixed inset-0 bg-gray-600 bg-opacity-75" />
        </TransitionChild>
        <TransitionChild as="template" enter="transition ease-in-out duration-300 transform" enter-from="-translate-x-full" enter-to="translate-x-0" leave="transition ease-in-out duration-300 transform" leave-from="translate-x-0" leave-to="-translate-x-full">
          <div class="relative flex-1 flex flex-col max-w-xs w-full bg-white focus:outline-none">
            <TransitionChild as="template" enter="ease-in-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in-out duration-300" leave-from="opacity-100" leave-to="opacity-0">
              <div class="absolute top-0 right-0 -mr-12 pt-2">
                <button type="button" class="ml-1 flex items-center justify-center h-10 w-10 rounded-full focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white" @click="sidebarOpen = false">
                  <span class="sr-only">Close sidebar</span>
                  <XIcon class="h-6 w-6 text-white" aria-hidden="true" />
                </button>
              </div>
            </TransitionChild>
            <div class="flex-1 h-0 pt-5 pb-4 overflow-y-auto">
              <div class="flex-shrink-0 flex items-center px-4">
                <img class="h-8 w-auto" src="https://tailwindui.com/img/logos/workflow-logo-indigo-600-mark-gray-900-text.svg" alt="Workflow" />
              </div>
              <nav aria-label="Sidebar" class="mt-5">
                <div class="px-2 space-y-1">
                  <a v-for="item in navigation" :key="item.name" :href="item.href" :class="[item.current ? 'bg-gray-100 text-gray-900' : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900', 'group flex items-center px-2 py-2 text-base font-medium rounded-md']">
                    <component :is="item.icon" :class="[item.current ? 'text-gray-500' : 'text-gray-400 group-hover:text-gray-500', 'mr-4 h-6 w-6']" aria-hidden="true" />
                    {{ item.name }}
                  </a>
                </div>
              </nav>
            </div>
          </div>
        </TransitionChild>
        <div class="flex-shrink-0 w-14" aria-hidden="true">
          <!-- Force sidebar to shrink to fit close icon -->
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Static sidebar for desktop -->
    <div class="hidden lg:flex lg:flex-shrink-0">
      <div class="flex flex-col w-64">
        <div class="flex-1 flex flex-col min-h-0 border-r border-gray-200 bg-gray-100">
          <div class="flex-1 flex flex-col pt-5 pb-4 overflow-y-auto">
            <div class="flex items-center flex-shrink-0 px-4">
              <img class="h-8 w-auto" src="/logo.png" alt="Workflow" />
            </div>
            <nav class="mt-5 flex-1" aria-label="Sidebar">
              <div class="px-2 space-y-1">
                <a v-for="item in navigation" :key="item.name" :href="item.href" :class="[item.current ? 'bg-gray-200 text-gray-900' : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900', 'group flex items-center px-2 py-2 text-sm font-medium rounded-md']">
                  <component :is="item.icon" :class="[item.current ? 'text-gray-500' : 'text-gray-400 group-hover:text-gray-500', 'mr-3 h-6 w-6']" aria-hidden="true" />
                  {{ item.name }}
                </a>
              </div>
            </nav>
          </div>
        </div>
      </div>
    </div>
    <div class="flex flex-col min-w-0 flex-1 overflow-hidden">
      <div class="lg:hidden">
        <div class="flex items-center justify-between bg-gray-50 border-b border-gray-200 px-4 py-1.5">
          <div>
            <img class="h-8 w-auto" src="https://tailwindui.com/img/logos/workflow-mark-indigo-600.svg" alt="Workflow" />
          </div>
          <div>
            <button type="button" class="-mr-3 h-12 w-12 inline-flex items-center justify-center rounded-md text-gray-500 hover:text-gray-900" @click="sidebarOpen = true">
              <span class="sr-only">Open sidebar</span>
              <MenuIcon class="h-6 w-6" aria-hidden="true" />
            </button>
          </div>
        </div>
      </div>
      <div class="flex-1 relative z-0 flex overflow-hidden">
        <main class="flex-1 relative z-0 overflow-y-auto focus:outline-none xl:order-last">
          <!-- Start main area-->
          <div class="absolute inset-0 py-6 px-4 sm:px-6 lg:px-8">
            <div class="revision">
              <h1 class="text-3xl md:text-4xl">Révision</h1>
              <div v-if="!revisionStarted" class="flex divide-x-4 p-6">
                <div class="pr-16">
                <h2 class="text-xl md:text-2xl mt-4 md:mt-8 ">Choix du thèmes :</h2>
                <li v-for="category in categories" :key="category.id" class="border border-black rounded-lg p-4 relative text-xl md:text-2xl mt-4 md:mt-8">
                  {{category.name}}
                  <div v-for="theme in category.themes" :key="theme.id" class=" p-4 relative">
                    <h3 class="text-xl md:text-2xl mt-4 md:mt-8 "><input :id="theme.id" v-model="selectedThemes" :value="theme" type="checkbox" class="mr-2 md:mr-4" :disabled="areAllCardsReviewedToday(theme)"/>» {{theme.name}}</h3>
                    <span v-if="areAllCardsReviewedToday(theme)" class="text-sm md:text-base text-gray-500 ml-2">
                        (Toutes les cartes de ce thème ont déjà été révisées aujourd'hui)
                      </span>

                  </div>
                </li>
                </div>
                <div v-if="selectedThemes.length > 0" class="border-blue-500 mt-4 pl-16 md:mt-8">
                  <h2 class="text-xl md:text-2xl">Nombre de niveaux :</h2>
                  <input
                      v-model.number="numberOfLevels"
                      max="10"
                      min="1"
                      type="number"
                      class="border-blue-400 rounded-md px-2 py-1 w-full md:w-2/5 mt-2 md:mt-4 border border-3"
                  />
                  <h2 class="text-xl md:text-2xl mt-4 md:mt-8">Nombre de nouvelles cartes par jour</h2>
                  <input
                      v-model.number="newCardsPerDay"
                      max="50"
                      min="1"
                      type="number"
                      class="border-blue-400 rounded-md px-2 py-1 w-full md:w-2/5 mt-2 md:mt-4 border border-3"
                  />
                  <br>
                  <button @click="showModal()"
                          class="bg-blue-500 text-white font-bold rounded-md py-2 px-4 mb-2 md:mb-0 hover:bg-blue-700 transition-colors duration-300">
                    Commencer la révision
                  </button>
                </div>
              </div>

<!--      TAB DE REVISION        -->
              <div v-if="revisionStarted">
                <div v-if="currentCard" class="card mt-4 md:mt-8" @click="showFront = !showFront">
                  <div :class="{ 'card-flip': !showFront }" class="card-content">
                    <h2 class="text-2xl md:text-3xl">{{ currentCard.recto }}</h2>
                  </div>
                  <div :class="{ 'card-flip': showFront }" class="card-content">
                    <h2 class="text-2xl md:text-3xl">{{ currentCard.verso }}</h2>
                    <div class="mt-4 md:mt-8">
                      <button @click.stop="cardAnswered(true)"
                              class="bg-green-500 text-white rounded-md py-2 px-4 mr-2 md:mr-4 hover:bg-green-700 transition-colors duration-300 p-3">
                        Oui, je me souviens
                      </button>
                      <button @click.stop="cardAnswered(false)"
                              class="bg-red-500 text-white rounded-md py-2 px-4 hover:bg-red-700 transition-colors duration-300">
                        Non, j'ai oublié
                      </button>
                    </div>
                  </div>
                </div>
                <div v-else class="bg-white rounded-lg shadow-md p-8 mx-auto max-w-xl">
                  <div class="flex flex-col sm:flex-row sm:items-center sm:justify-center">
                    <h1 class="font-bold m-3">Révision terminée !</h1>
                    <button class="bg-yellow-400 hover:bg-yellow-600 text-white font-bold py-2 px-4 rounded mb-4 sm:mr-4"
                            @click="scheduleDailyReminder">Planifier un rappel quotidien
                    </button>
                    <button class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded mb-4">
                      <router-link :to="`/`">Retourner à l'accueil</router-link>
                    </button>
                  </div>
                </div>
              </div>
<!--              FIN TAB DE REVISION       -->
            </div>
          </div>
          <!-- End main area -->
        </main>
      </div>
    </div>
  </div>


  <!-- The Modal -->
  <div id="readyModal" class="modal">
    <div class="fixed inset-0 z-10 overflow-y-auto">
      <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
        <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
          <div class="bg-white px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                <h3 class="text-2xl font-semibold leading-6 text-gray-900">Révision</h3>
                <div class="mt-2">
                  <p class="text-sm text-gray-500">Es-tu pret a commencer une révision sur les themes suivant :</p>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
            <button type="button" class="inline-flex w-full justify-center rounded-md bg-blue-950 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-blue-900 sm:ml-3 sm:w-auto" @click="startRevision">Go !</button>
            <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="closeModal()" ref="cancelButtonRef">Fermer</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- The Modal -->
  <div id="errorModal" class="modal">
    <div class="fixed inset-0 z-10 overflow-y-auto">
      <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
        <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
          <div class="bg-gray-100 px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                !
              </div>
              <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                <h3 class="text-2xl font-semibold leading-6 text-gray-900">Erreur</h3>
                <div class="mt-2">
                  <p class="text-sm text-gray-500">Malheureusement, il semblerait qu'il n'y ait aucune carte dans le/les theme(s) sélectionné(s).</p>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
            <a type="button" class="inline-flex w-full justify-center rounded-md bg-blue-950 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-blue-900 sm:ml-3 sm:w-auto" href="/">Ajouter une carte</a>
            <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="closeModal" ref="cancelButtonRef">Fermer</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {computed, ref} from 'vue';
import {useStore} from '@/stores/counter';
import {format} from 'date-fns';
import { Dialog, DialogOverlay, TransitionChild, TransitionRoot } from '@headlessui/vue'
import {
  HomeIcon,
  MenuIcon,
  SearchCircleIcon,
  XIcon,
} from '@heroicons/vue/outline'
const navigation = [
  { name: 'Accueil', href: '/', icon: HomeIcon, current: false },
  { name: 'Révision', href: '/revision', icon: SearchCircleIcon, current: true },
]

export default {
    name: 'Revision',
    components: {
      Dialog,
      DialogOverlay,
      TransitionChild,
      TransitionRoot,
      MenuIcon,
      XIcon,
    },
    setup() {
        const store = useStore();
        const allThemes = computed(() => store.getAllThemes());
        const categories = ref(store.$state.categories);
        const selectedThemes = ref([]);
        const numberOfLevels = ref(1);
        const newCardsPerDay = ref(5);
        const revisionStarted = ref(false);
        const revisionCards = ref([]);
        const currentCardIndex = ref(0);
        const showFront = ref(true);
        const revisionInterval = ref(null);
        const sidebarOpen = ref(false)

      console.log(categories)
        let openrdy = false;
        let openerror = false;
        function showModal() {
          const selectedThemesHaveCards = selectedThemes.value && selectedThemes.value.every(theme => theme.cards && theme.cards.length > 0);
            if (!selectedThemesHaveCards) {
              const errorModal = document.getElementById("errorModal");
              errorModal.style.display = "block";
              selectedThemes.value = [];
              openerror = true;
            } else {
              const readyModal = document.getElementById("readyModal");
              readyModal.style.display = "block";
              openrdy = true;
            }
        }

        function closeModal(){
          if (openrdy){
            const readyModal = document.getElementById("readyModal");
            readyModal.style.display = "none";
          }
          if (openerror){
            const errorModal = document.getElementById("errorModal");
            errorModal.style.display = "none";
          }
        }

        function startRevision() {
          revisionStarted.value = true;
          revisionCards.value = getRevisionCards();
        }

        function getRevisionCards() {
            const cards = selectedThemes.value.flatMap((theme) => {
                return theme.cards.filter((card) => {
                    const currentDate = new Date();
                    const lastRevisionDate = card.lastRevisionDate
                        ? new Date(card.lastRevisionDate)
                        : new Date(0);
                    const daysSinceLastRevision = Math.floor(
                        (currentDate - lastRevisionDate) / (1000 * 60 * 60 * 24)
                    );
                    return card.level <= numberOfLevels.value && daysSinceLastRevision >= card.level;
                });
            });
            const revisionCardsSet = new Set(cards);

            for (let i = numberOfLevels.value; i >= 1; i--) {
                const levelCards = cards.filter((card) => card.level === i);
                levelCards.sort((a, b) => b.lastRevisionDate - a.lastRevisionDate);
                for (const card of levelCards) {
                    if (revisionCardsSet.size >= newCardsPerDay.value) break;
                    revisionCardsSet.add(card);
                }
                if (revisionCardsSet.size >= newCardsPerDay.value) break;
            }
            for (let i = 1; i <= numberOfLevels.value; i++) {
                const levelCards = cards.filter((card) => card.level === i);
                levelCards.sort((a, b) => b.lastRevisionDate - a.lastRevisionDate);
                for (const card of levelCards) {
                    if (revisionCardsSet.size >= newCardsPerDay.value) break;
                    revisionCardsSet.add(card);
                }
                if (revisionCardsSet.size >= newCardsPerDay.value) break;
            }
            return [...revisionCardsSet];
        }

        function cardAnswered(remembered) {
            const currentCard = revisionCards.value[currentCardIndex.value];
            if (!currentCard) {
                return;
            }
            const newLevel = remembered
                ? Math.min(currentCard.level + 1, numberOfLevels.value)
                : Math.max(currentCard.level = 1, 1);

            store.updateCardLevel(
                currentCard.categoryId,
                currentCard.themeId,
                currentCard.id,
                newLevel
            );
            store.updateCardLastRevisionDate(
                currentCard.categoryId,
                currentCard.themeId,
                currentCard.id,
                new Date()
            );

            currentCard.level = newLevel;
            currentCard.lastRevisionDate = new Date();
            currentCardIndex.value++;
            showFront.value = true;

            if (currentCardIndex.value < revisionCards.value.length) {
                const nextCard = revisionCards.value[currentCardIndex.value];
                nextCard.level = newLevel;
                nextCard.lastRevisionDate = new Date();
                revisionCards.value.splice(currentCardIndex.value, 1, nextCard);
            } else {
                clearInterval(revisionInterval.value);
                revisionInterval.value = null;
                revisionCards.value = [];
                currentCardIndex.value = 0;
                showFront.value = true;
            }
        }

        const currentCard = computed(() => {
            return revisionCards.value[currentCardIndex.value] || null;
        });

        function areAllCardsReviewedToday(theme) {
            const currentDate = new Date();
            if (theme.cards !== undefined) {
              return theme.cards.every((card) => {
                const lastRevisionDate = card.lastRevisionDate
                    ? new Date(card.lastRevisionDate)
                    : new Date(0);
                return (
                    currentDate.getFullYear() === lastRevisionDate.getFullYear() &&
                    currentDate.getMonth() === lastRevisionDate.getMonth() &&
                    currentDate.getDate() === lastRevisionDate.getDate()
                );
              });
            } else return false
        }

        function scheduleDailyReminder() {
            if (Notification.permission !== 'granted') {
                Notification.requestPermission().then((permission) => {
                    if (permission === 'granted') {
                        console.log('Notification permission granted.');
                        scheduleReminder();
                    }
                });
            } else {
                scheduleReminder();
            }

            function scheduleReminder() {
                const now = new Date();
                const reminderTime = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 10, 0, 0);
                if (now.getTime() > reminderTime.getTime()) {
                    reminderTime.setDate(reminderTime.getDate() + 1);
                }
                const selectedTime = window.prompt(
                    'À quelle heure souhaitez-vous être notifié(e) chaque jour ? (h:min)',
                    format(reminderTime, 'HH:mm')
                );
                if (selectedTime) {
                    const [hour, minute] = selectedTime.split(':');
                    reminderTime.setHours(parseInt(hour, 10));
                    reminderTime.setMinutes(parseInt(minute, 10));
                    const delay = reminderTime.getTime() - new Date().getTime();
                    setInterval(() => {
                        const notification = new Notification('Rappel quotidien', {
                            body: `Il est ${selectedTime}, c'est l'heure de réviser tes cartes pour aujourd'hui !`,
                        });
                    }, delay);
                }
            }
        }

        return {
            allThemes,
            selectedThemes,
            numberOfLevels,
            newCardsPerDay,
            revisionStarted,
            currentCard,
            showFront,
            startRevision,
            cardAnswered,
            areAllCardsReviewedToday,
            scheduleDailyReminder,
            navigation,
            sidebarOpen,
            showModal,
            closeModal,
            categories
        };
    },
};
</script>

<style scoped>

/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

.revision {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.theme {
    display: flex;
    align-items: center;
}

input[type='number'] {
    width: 50px;
    margin-bottom: 20px;
}

.card {
    perspective: 1000px;
    width: 400px;
    height: 200px;
    border: 1px solid #ccc;
    border-radius: 10px;
    background-color: white;
    cursor: pointer;
}

.card-content {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    transition: transform 0.6s;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.card-flip {
    transform: rotateY(180deg);
}

</style>
