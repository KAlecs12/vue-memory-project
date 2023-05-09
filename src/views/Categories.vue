<template>
  <div class="h-screen flex">
    <TransitionRoot as="template" :show="sidebarOpen">
      <Dialog as="div" class="fixed inset-0 flex z-40 lg:hidden" @close="sidebarOpen = false">
        <TransitionChild as="template" enter="transition-opacity ease-linear duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="transition-opacity ease-linear duration-300" leave-from="opacity-100" leave-to="opacity-0">
          <DialogOverlay class="fixed inset-0 bg-gray-600 bg-opacity-75" />
        </TransitionChild>
        <TransitionChild as="template" enter="transition ease-in-out duration-300 transform" enter-from="-translate-x-full" enter-to="translate-x-0" leave="transition ease-in-out duration-300 transform" leave-from="translate-x-0" leave-to="-translate-x-full">
          <div class="flex-1 flex flex-col max-w-xs w-full bg-white focus:outline-none">
            <TransitionChild as="template" enter="ease-in-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in-out duration-300" leave-from="opacity-100" leave-to="opacity-0">
              <div class="top-0 right-0 -mr-12 pt-2">
                <button type="button" class="ml-1 flex items-center justify-center h-10 w-10 rounded-full focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white" @click="sidebarOpen = false">
                  <span class="sr-only">Close sidebar</span>
                  <XIcon class="h-6 w-6 text-white" aria-hidden="true" />
                </button>
              </div>
            </TransitionChild>
            <div class="flex-1 h-0 pt-5 pb-4 h-full">
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
        <!-- Sidebar component, swap this element with another sidebar if you like -->
        <div class="flex-1 flex flex-col min-h-0 border-r border-gray-200 bg-gray-100">
          <div class="flex-1 flex flex-col pt-5 pb-4 overflow-y-auto">
            <div class="flex items-center flex-shrink-0 px-4">
              <img class="h-8 w-auto" src="https://tailwindui.com/img/logos/workflow-logo-indigo-600-mark-gray-900-text.svg" alt="Workflow" />
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
            <div class="container mx-auto px-4 sm:px-6 lg:px-8">
              <h3 class="text-2xl font-bold overflow-y-auto h-10 flex justify-center mb-6">Toutes les catégories</h3>
              <ul class="grid grid-cols-1 sm:grid-cols-2 gap-6 lg:grid-cols-3">
                <li v-for="category in categories" :key="category.id"
                    class="border border-8 border-indigo-400 rounded-lg p-4 relative">
                  <div class="content">
                    <div v-if="category.id !== editedCategoryId">
                      <!--      EDIT DELETE       -->
                      <div class="flex justify-between">
                        <button @click="editCategory(category.id)"
                                class="bg-yellow-500 hover:bg-yellow-700 py-2 px-4 rounded-lg font-bold text-white">
                          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M11.42 15.17L17.25 21A2.652 2.652 0 0021 17.25l-5.877-5.877M11.42 15.17l2.496-3.03c.317-.384.74-.626 1.208-.766M11.42 15.17l-4.655 5.653a2.548 2.548 0 11-3.586-3.586l6.837-5.63m5.108-.233c.55-.164 1.163-.188 1.743-.14a4.5 4.5 0 004.486-6.336l-3.276 3.277a3.004 3.004 0 01-2.25-2.25l3.276-3.276a4.5 4.5 0 00-6.336 4.486c.091 1.076-.071 2.264-.904 2.95l-.102.085m-1.745 1.437L5.909 7.5H4.5L2.25 3.75l1.5-1.5L7.5 4.5v1.409l4.26 4.26m-1.745 1.437l1.745-1.437m6.615 8.206L15.75 15.75M4.867 19.125h.008v.008h-.008v-.008z" />
                          </svg>
                        </button>
                        <button @click="deleteCategory(category.id)" class="bg-red-500 hover:bg-red-700 py-2 px-4 rounded-lg font-bold text-white">
                          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                          </svg>
                        </button>
                      </div>

                      <h3 class="text-lg font-bold overflow-y-auto h-10 flex justify-center">{{ category.name }}</h3>

                      <div class="flex justify-between flex-col sm:flex-row sm:items-center space-y-4 sm:space-y-0 sm:space-x-4">
                        <p class="mb-4 text-base overflow-y-auto h-1/6">{{ category.description }}</p>
                      </div>
                      <router-link :to="`/categories/${category.id}/`" class="bg-blue-500 hover:bg-blue-700 py-2 rounded-lg font-bold text-white">
                        <button class="w-full">Ouvrir
                          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="inline w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 9.776c.112-.017.227-.026.344-.026h15.812c.117 0 .232.009.344.026m-16.5 0a2.25 2.25 0 00-1.883 2.542l.857 6a2.25 2.25 0 002.227 1.932H19.05a2.25 2.25 0 002.227-1.932l.857-6a2.25 2.25 0 00-1.883-2.542m-16.5 0V6A2.25 2.25 0 016 3.75h3.879a1.5 1.5 0 011.06.44l2.122 2.12a1.5 1.5 0 001.06.44H18A2.25 2.25 0 0120.25 9v.776" />
                          </svg>
                        </button>
                      </router-link>
                    </div>

                    <form v-else @submit.prevent="updateCategory" class="flex flex-col items-center">
                      <input v-model="editedCategory.name" placeholder="Nom" required
                             class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"/>
                      <textarea v-model="editedCategory.description" placeholder="Description" required
                                class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"
                                rows="3"></textarea>
                      <div class="flex flex-col sm:flex-row justify-center sm:justify-end sm:items-center space-y-4 sm:space-y-0 sm:space-x-4">
                        <button type="submit"
                                class="bg-blue-500 hover:bg-blue-700 py-2 px-4 rounded-lg font-bold text-white">
                          Enregistrer
                        </button>
                        <button @click="cancelEdit"
                                class="bg-gray-500 hover:bg-gray-400 py-2 px-4 rounded-lg font-bold text-white">
                          Annuler
                        </button>
                      </div>
                    </form>
                  </div>
                </li>
                <button @click="showModal" type="button" class="relative block w-full border-2 border-gray-300 border-dashed rounded-lg p-12 text-center hover:border-gray-400 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"  class="mx-auto h-12 w-12 text-gray-400">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M7.875 14.25l1.214 1.942a2.25 2.25 0 001.908 1.058h2.006c.776 0 1.497-.4 1.908-1.058l1.214-1.942M2.41 9h4.636a2.25 2.25 0 011.872 1.002l.164.246a2.25 2.25 0 001.872 1.002h2.092a2.25 2.25 0 001.872-1.002l.164-.246A2.25 2.25 0 0116.954 9h4.636M2.41 9a2.25 2.25 0 00-.16.832V12a2.25 2.25 0 002.25 2.25h15A2.25 2.25 0 0021.75 12V9.832c0-.287-.055-.57-.16-.832M2.41 9a2.25 2.25 0 01.382-.632l3.285-3.832a2.25 2.25 0 011.708-.786h8.43c.657 0 1.281.287 1.709.786l3.284 3.832c.163.19.291.404.382.632M4.5 20.25h15A2.25 2.25 0 0021.75 18v-2.625c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125V18a2.25 2.25 0 002.25 2.25z" />
                  </svg>
                  <span class="mt-2 block text-sm font-medium text-gray-900"> Ajouter une nouvelle catégorie </span>
                </button>
              </ul>
            </div>
          </div>
          <!-- End main area -->
        </main>
      </div>
    </div>
  </div>

  <!-- The Modal -->
  <div id="addCategory" class="modal">
    <div class="fixed inset-0 z-10 overflow-y-auto">
      <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
        <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
          <div class="bg-gray-100 px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                <h3 class="text-2xl font-semibold leading-6 text-gray-900 flex justify-center">Catégorie</h3>
                <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="closeModal" ref="cancelButtonRef"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 12h-15m0 0l6.75 6.75M4.5 12l6.75-6.75" /></svg></button>

                <form @submit.prevent="addCategory" class="max-w-md mx-auto my-10">
                  <input v-model="newCategory.name" placeholder="Nom" required
                         class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"/>
                  <textarea v-model="newCategory.description" placeholder="Description . . ." required
                            class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"
                            rows="3"></textarea>
                  <button type="submit"
                          class="bg-blue-950 hover:bg-blue-900 text-white font-bold py-2 px-4 rounded-lg font-bold text-base w-full">
                    Ajouter une catégorie
                  </button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import {useStore} from '@/stores/counter';
import {RouterLink, useRouter} from 'vue-router';

import {ref, watch} from 'vue'
import { Dialog, DialogOverlay, TransitionChild, TransitionRoot } from '@headlessui/vue'
import {
  SearchCircleIcon,
  HomeIcon,
  MenuIcon,
  XIcon,
} from '@heroicons/vue/outline'

const navigation = [
  { name: 'Accueil', href: '/', icon: HomeIcon, current: true },
  { name: 'Révision', href: '/revision', icon: SearchCircleIcon, current: false },
]

export default {
  components: {
    Dialog,
    DialogOverlay,
    TransitionChild,
    TransitionRoot,
    MenuIcon,
    XIcon,
    RouterLink
  },
  setup() {
    const sidebarOpen = ref(false);
    const router = useRouter();
    const store = useStore();
    const categories = ref(store.$state.categories);

    watch(
        () => store.$state.categories,
        (newCategories) => {
          categories.value = newCategories;
        },
        {deep: true}
    );

    let openadd = false;
    function showModal() {
        const addCategory = document.getElementById("addCategory");
        addCategory.style.display = "block";
        openadd = true;
    }

    function closeModal(){
      if (openadd){
        const addCategory = document.getElementById("addCategory");
        addCategory.style.display = "none";
      }
    }

    const newCategory = ref({
      id: null,
      name: '',
      description: '',
      themes: [],
    });

    const editedCategoryId = ref(null);
    const editedCategory = ref({
      id: null,
      name: '',
      description: '',
      themes: [],
    });

    const addCategory = () => {
      const addCategory = document.getElementById("addCategory");
      addCategory.style.display = "none";
      newCategory.value.id = Date.now();
      store.addCategory(newCategory.value);
      newCategory.value = {
        id: null,
        name: '',
        description: '',
        themes: [],
      };
    };

    const editCategory = (id) => {
      const category = store.$state.categories.find((category) => category.id === id);
      editedCategory.value = {...category};
      editedCategoryId.value = id;
    };

    const updateCategory = () => {
      store.updateCategory(editedCategory.value);
      editedCategoryId.value = null;
    };

    const deleteCategory = (id) => {
      store.deleteCategory(id);
    };

    const cancelEdit = () => {
      editedCategoryId.value = null;
    };

    return {
      navigation,
      sidebarOpen,
      categories,
      newCategory,
      addCategory,
      editedCategoryId,
      editedCategory,
      editCategory,
      updateCategory,
      deleteCategory,
      cancelEdit,
      router,
      showModal,
      closeModal
    };
  },
}
</script>

<style>

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
</style>