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
              <form @submit.prevent="addCategory" class="max-w-md mx-auto my-10">
                <input v-model="newCategory.name" placeholder="Nom" required
                       class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"/>
                <textarea v-model="newCategory.description" placeholder="Description" required
                          class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"
                          rows="3"></textarea>
                <button type="submit"
                        class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-lg font-bold text-base w-full">
                  Ajouter une catégorie
                </button>
              </form>

              <ul class="grid grid-cols-1 sm:grid-cols-2 gap-6 lg:grid-cols-3">
                <li v-for="category in categories" :key="category.id"
                    class="border border-gray-300 rounded-lg p-8 relative">
                  <div class="content">
                    <div v-if="category.id !== editedCategoryId">
                      <h3 class="mb-4 text-lg font-bold overflow-y-auto h-10">{{ category.name }}</h3>
                      <p class="mb-4 text-base overflow-y-auto h-10">{{ category.description }}</p>
                      <div class="flex flex-col sm:flex-row justify-center sm:justify-end sm:items-center space-y-4 sm:space-y-0 sm:space-x-4">
                        <button @click="editCategory(category.id)"
                                class="bg-yellow-500 hover:bg-yellow-700 py-2 px-4 rounded-lg font-bold text-white">
                          Modifier
                        </button>
                        <button @click="deleteCategory(category.id)"
                                class="bg-red-500 hover:bg-red-700 py-2 px-4 rounded-lg font-bold text-white">
                          Supprimer
                        </button>
                        <button>
                          <router-link :to="`/categories/${category.id}/`"
                                       class="bg-blue-500 hover:bg-blue-700 py-2 px-4 rounded-lg font-bold text-white">
                            Voir
                          </router-link>
                        </button>
                      </div>
                    </div>

                    <form v-else @submit.prevent="updateCategory" class="flex flex-col items-center">
                      <input v-model="editedCategory.name" placeholder="Nom" required
                             class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"/>
                      <textarea v-model="editedCategory.description" placeholder="Description" required
                                class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans"
                                rows="3"></textarea>
                      <div class="flex flex-col sm:flex-row justify-center sm:justify-end sm:items-center space-y-4 sm:space-y-0 sm:space-x-4">
                        <button type="submit"
                                class="bg-green-500 hover:bg-green-600 py-2 px-4 rounded-lg font-bold text-white">
                          Enregistrer
                        </button>
                        <button @click="cancelEdit"
                                class="bg-gray-300 hover:bg-gray-400 py-2 px-4 rounded-lg font-bold text-white">
                          Annuler
                        </button>
                      </div>
                    </form>
                  </div>
                </li>
              </ul>
            </div>
          </div>
          <!-- End main area -->
        </main>
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
    };
  },
}
</script>