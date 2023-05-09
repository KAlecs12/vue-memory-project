<template>
    <div>
        <div class="container mx-auto pt-8 px-4 lg:px-0">
            <h1 class="text-3xl font-bold overflow-y-auto h-10">{{ category.name }}</h1>
            <p class="mt-2 overflow-y-auto h-10">{{ category.description }}</p>
          <button type="button" class="bg-blue-500 hover:bg-blue-700 mt-3 inline-flex w-full justify-center rounded-md px-3 py-2 text-sm font-semibold text-white shadow-sm ring-1 ring-inset ring-gray-300 sm:mt-0 sm:w-auto" @click="goBack" ref="cancelButtonRef"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 12h-15m0 0l6.75 6.75M4.5 12l6.75-6.75" /></svg></button>
        </div>



        <h2 class="flex justify-center text-2xl font-bold container mb-8 mx-auto px-4 lg:px-0">Thèmes</h2>


        <div class=" pl-16 pr-16 flex flex-col">
          <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
            <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
              <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
                <table class="min-w-full divide-y divide-gray-200">
                  <thead class="bg-gray-50">
                  <tr>
                    <th scope="col" class="px-6 py-3 text-left text-xl font-medium text-gray-500 uppercase tracking-wider">Nom</th>
                    <th scope="col" class="py-3 text-xl font-medium text-gray-500 uppercase tracking-wider">Nombre de Carte</th>
                    <th scope="col" class="relative px-6 py-3">
                      <span class="sr-only">Edit</span>
                    </th>
                  </tr>
                  </thead>
                  <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="theme in category.themes" :key="theme.id">
                    <td class="px-6 py-4 whitespace-nowrap">
                      <div class="flex items-center">
                        <div class="ml-4">
                          <div class="text-xl font-medium text-gray-900">
                            <router-link :to="'/categories/' + categoryId + '/themes/' + theme.id">
                              <button>
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="inline w-6 h-6">
                                  <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 9.776c.112-.017.227-.026.344-.026h15.812c.117 0 .232.009.344.026m-16.5 0a2.25 2.25 0 00-1.883 2.542l.857 6a2.25 2.25 0 002.227 1.932H19.05a2.25 2.25 0 002.227-1.932l.857-6a2.25 2.25 0 00-1.883-2.542m-16.5 0V6A2.25 2.25 0 016 3.75h3.879a1.5 1.5 0 011.06.44l2.122 2.12a1.5 1.5 0 001.06.44H18A2.25 2.25 0 0120.25 9v.776" />
                                </svg>
                              </button>
                            </router-link>

                            {{ theme.name }}
                          </div>
                        </div>
                      </div>
                    </td>
                    <td class="flex justify-center py-4 whitespace-nowrap">
                      <span class="px-4 py-2 inline-flex text-xl leading-5 font-semibold rounded-full bg-indigo-400 text-white" v-if="theme.cards === undefined">0</span>
                      <span class="px-4 py-2 inline-flex text-xl leading-5 font-semibold rounded-full bg-indigo-400 text-white" v-if="theme.cards && theme.cards.length > 0"> {{theme.cards.length}}</span>
                    </td>

                    <td class="px-6 py-4 text-right text-sm font-medium">
                      <a class="text-indigo-600 hover:text-indigo-900" @click="showModal('Modif', theme)">Modifier</a>
                      <br>
                      <a class="text-indigo-600 hover:text-indigo-900" @click="showModal('Suppr', theme)" >Supprimer</a>
                    </td>
                  </tr>
                  </tbody>
                </table>
                <button @click="showModal('Add')" type="button" class="relative block w-full border-2 border-gray-300 border-dashed rounded-lg p-12 text-center hover:border-gray-400 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="mx-auto h-12 w-12 text-gray-400">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M9.568 3H5.25A2.25 2.25 0 003 5.25v4.318c0 .597.237 1.17.659 1.591l9.581 9.581c.699.699 1.78.872 2.607.33a18.095 18.095 0 005.223-5.223c.542-.827.369-1.908-.33-2.607L11.16 3.66A2.25 2.25 0 009.568 3z" />
                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 6h.008v.008H6V6z" />
                  </svg>
                  <span class="mt-2 block text-sm font-medium text-gray-900"> Ajouter un nouveau theme </span>
                </button>
              </div>
            </div>
          </div>
        </div>

      <!-- The MODIF -->
      <div id="modifModal" class="modal">
        <div class="fixed inset-0 z-10 overflow-y-auto">
          <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
            <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
              <div class="bg-gray-100 px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
                <div class="flex justify-center ">
                  <div class="mb-4 mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                    <h3 class="flex justify-center text-2xl font-semibold leading-6 text-gray-900">Modification du nom :</h3>
                    <div class="mt-2">
                      <form class="flex flex-col mt-4" @submit.prevent="updateTheme">
                        <input class="border border-gray-300 p-2 rounded-md mb-2" v-model="editedTheme.name" placeholder="Nom" required/>
                        <div class="mt-4 flex flex-row justify-center items-center">
                          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mr-2" type="submit">
                            Enregistrer
                          </button>
                          <button class="bg-gray-500 hover:bg-gray-400 text-white font-bold py-2 px-4 rounded" @click="cancelEdit">
                            Annuler
                          </button>
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- The SUPPR -->
      <div id="deleteModal" class="modal">
        <div class="fixed inset-0 z-10 overflow-y-auto">
          <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
            <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
              <div class="bg-white px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
                <div class="sm:flex sm:items-start">
                  <div class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                    !
                  </div>
                  <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                    <h3 class="text-2xl font-semibold leading-6 text-gray-900">SUPPRIMER</h3>
                    <div class="mt-2">
                      <p class="text-sm text-gray-500">Es-tu vraiment sur de vouloir supprimer ce theme ? </p>
                    </div>
                  </div>
                </div>
              </div>
              <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
                <button type="button" class="inline-flex w-full justify-center rounded-md bg-blue-950 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-blue-900 sm:ml-3 sm:w-auto" @click="deleteTheme(currentId)">Supprimer !</button>
                <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="closeModal()" ref="cancelButtonRef">Fermer</button>
              </div>
            </div>
          </div>
        </div>
      </div>


      <!-- The ADD -->
      <div id="addModal" class="modal">
        <div class="fixed inset-0 z-10 overflow-y-auto">
          <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
            <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
              <div class="bg-gray-100 px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
                <div class="sm:flex justify-center">
                  <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                    <h3 class="text-2xl font-semibold leading-6 text-gray-900 flex justify-center">Ajouter un theme</h3>
                    <button type="button" class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="closeModal" ref="cancelButtonRef"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 12h-15m0 0l6.75 6.75M4.5 12l6.75-6.75" /></svg></button>

                    <form class="max-w-md mx-auto my-10" @submit.prevent="addTheme">
                      <input  class="w-full mb-4 px-4 py-2 border border-gray-300 rounded-lg text-base font-sans" v-model="newTheme.name" placeholder="Nom" required/>
                      <button  class="bg-blue-950 hover:bg-blue-900 text-white font-bold py-2 px-4 rounded-lg font-bold text-base w-full" type="submit">
                        Ajouter un thème
                      </button>
                    </form>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        </div>
    </div>

  <div id="addCategory" class="modal">
    <div class="fixed inset-0 z-10 overflow-y-auto">
      <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
        <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
          <div class="bg-gray-100 px-4 pb-4 pt-5 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mt-3 text-center sm:ml-4 sm:mt-0 sm:text-left">
                <h3 class="text-2xl font-semibold leading-6 text-gray-900 flex justify-center">Ajouter un theme</h3>
                <button type="button" class="mt-3 inline-flex w-full  rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" @click="closeModal" ref="cancelButtonRef"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 12h-15m0 0l6.75 6.75M4.5 12l6.75-6.75" /></svg></button>

                <form class="flex flex-col mt-4" @submit.prevent="addTheme">
                  <input class="border border-gray-300 p-2 rounded-md mb-2" v-model="newTheme.name" placeholder="Nom" required/>
                  <button class="bg-blue-950 hover:bg-blue-900 rounded-lg  text-white font-bold py-2 px-4" type="submit">
                    Ajouter un thème
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
import {defineComponent, ref} from 'vue'
import {useRoute, useRouter} from 'vue-router'
import {useStore} from '@/stores/counter'

export default defineComponent({
    setup() {
        const route = useRoute()
        const categoryId = Number(route.params.id)
        const store = useStore()
        const router = useRouter()

        const category = ref(null)

        let opendelete = false;
        let openmodif = false;
        let openadd = false;
        let currentId;
      function showModal(string, theme) {
        if(theme) currentId = theme.id

        if (string === "Suppr") {
          const deleteModal = document.getElementById("deleteModal");
          deleteModal.style.display = "block";
          opendelete = true;
        } else if (string === "Modif") {
          const modifModal = document.getElementById("modifModal");
          modifModal.style.display = "block";
          editTheme(currentId)
          openmodif = true;
        } else if (string === "Add") {
          const addModal = document.getElementById("addModal");
          addModal.style.display = "block";
          openadd = true;
        }

      }

      function closeModal(){
        if (opendelete){
          const deleteModal = document.getElementById("deleteModal");
          deleteModal.style.display = "none";
          opendelete = false;
        }
        if (openmodif){
          const modifModal = document.getElementById("modifModal");
          modifModal.style.display = "none";
        }
        if (openadd){
          const addModal = document.getElementById("addModal");
          addModal.style.display = "none";
        }
      }

        const getCategoryById = (id) => {
            return store.categories.find((category) => category.id === id)
        }

        category.value = getCategoryById(categoryId)
        const newTheme = ref({
            id: null,
            name: '',
        })

        const editedThemeId = ref(null)
        const editedTheme = ref({
            id: null,
            name: '',
        })

        const addTheme = () => {
            const addModal = document.getElementById("addModal");
            addModal.style.display = "none";
            store.addTheme(categoryId, newTheme.value)
            newTheme.value = {
                id: null,
                name: '',
            }
        }

        const editTheme = (id) => {
            const theme = store.getThemeById(categoryId, id)
            editedTheme.value = {...theme}
            editedThemeId.value = id
        }

        const updateTheme = () => {
            const modifModal = document.getElementById("modifModal");
            modifModal.style.display = "none";
            store.updateTheme(categoryId, editedTheme.value)
            editedThemeId.value = null
        }

        const deleteTheme = () => {
            const deleteModal = document.getElementById("deleteModal");
            deleteModal.style.display = "none";
            store.deleteTheme(categoryId, currentId)
        }

        const cancelEdit = () => {
            editedThemeId.value = null
        }

        const goBack = () => {
            router.go(-1)
        }

        return {
            category,
            categoryId,
            newTheme,
            addTheme,
            editedThemeId,
            editedTheme,
            editTheme,
            updateTheme,
            deleteTheme,
            cancelEdit,
            goBack,
            showModal,
            closeModal,
            currentId
        }
    }
})
</script>
