<template>
  <div class="container mx-auto">
    <div class="flex flex-col items-center justify-center h-screen">
      <div class="w-full flex flex-row flex-nowrap flex-row-reverse place-content-center gap-4">
        <div class="self-center">
          <h2 class="text-center text-4xl font-bold tracking-tight text-white">
            octobucket
          </h2>
        </div>
        <div>
          <img
            src="/octopus.png"
            width="80"
            height="80"
            alt="Imagem de um polvo"
          >
        </div>
      </div>
      <div class="m-8">
        <h2 class="mt-6 text-center text-2xl font-bold tracking-tight text-white">
          The all-in-one bitbucket tool plataform.
        </h2>
        <p class="mt-4">
          Navigation in your bitbucket with a tree in your browser , search files and IA to writter better pull requests.
        </p>
      </div>
      <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
        <div class="bg-white py-6 px-4 shadow rounded-lg sm:px-8">
          <form class="space-y-4" method="post" @submit="OnSubmit">
            <div>
              <label for="email" class="block text-sm font-medium text-gray-700">Email</label><div class="mt-1">
                <input
                  id="email"
                  v-model="email"
                  name="email"
                  type="email"
                  autocomplete="email"
                  class="block w-full  rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
                >
              </div>
              <div v-if="showErrors">
                <p class="text-gray-700">
                  <b>Please correct the following error(s):</b>
                  <ul>
                    <li v-for="error in errors">
                      {{ error }}
                    </li>
                  </ul>
                </p>
              </div>
            </div><div>
              <button type="submit" class="flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
                Request Early Access
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  data: function () {
    return {
      errors: [''],
      email: null
    }
  },

  computed: {
    showErrors () {
      return this.errors.length > 1
    }
  },

  methods: {
    OnSubmit (e: { preventDefault: () => void }) {
      e.preventDefault()

      const baseUrl = `https://api.convertkit.com/v3/forms/${this.$config.formId}/subscribe`
      this.errors = ['']
      if (!this.email) {
        this.errors.push('E-mail is empty')
        return
      }
      try {
        this.$axios.post(baseUrl, { api_key: this.$config.apiKey, email: this.email, tags: ['octobucket'] })
      } catch (e :any) {
        this.errors.push('Invalid Error')
      }
    }
  }

})
</script>
