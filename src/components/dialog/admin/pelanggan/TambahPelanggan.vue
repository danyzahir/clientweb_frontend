<script setup>
import { reactive } from 'vue'

// Props method dari store (addData)
const props = defineProps({
  method: Function,
})

// Form reactive
const form = reactive({
  email: '',
  password: '',
  nama_pelanggan: '',
  alamat: '',
  no_hp: '',
})

async function submit() {
  try {
    // Validasi frontend
    if (!form.email || !form.password) {
      alert('Email dan password wajib diisi')
      return
    }

    // Kirim langsung objek (BUKAN FormData!)
    await props.method({ ...form })

    // Reset form
    form.email = ''
    form.password = ''
    form.nama_pelanggan = ''
    form.alamat = ''
    form.no_hp = ''
  } catch (error) {
    if (error.response?.data?.errors) {
      const messages = Object.values(error.response.data.errors).flat()
      alert(messages.join('\n'))
    } else {
      alert('Terjadi kesalahan saat submit')
    }
    console.error('Submit error:', error)
  }
}
</script>
<template>
  <v-dialog class="max-w-xl" :attach="null">
    <template v-slot:activator="{ props: activatorProps }">
      <button
        v-bind="activatorProps"
        class="!bg-secondary-3 text-white !text-sm px-5 py-2 rounded-lg hover:!bg-secondary-3/80 transition-all duration-300"
      >
        Tambah Pelanggan
      </button>
    </template>

    <template v-slot:default="{ isActive }">
      <div class="bg-white dark:!bg-dark-primary-1 !p-5 border dark:!border-white rounded-lg w-full">
        <div class="flex border-b dark:!border-typography-2 items-center justify-between px-3 pt-1 !pb-3">
          <p class="text-lg dark:!text-white">Tambah Pelanggan</p>
          <button @click="isActive.value = false">
            <i class="fa-solid fa-x dark:!text-white"></i>
          </button>
        </div>
        <div class="!px-3 !pt-3">
          <form @submit.prevent="submit">
            <div class="!space-y-3">

              <!-- Urutan diperbaiki -->
              <div>
                <label class="block mb-1 text-start dark:!text-white text-sm">Nama Pelanggan</label>
                <input
                  type="text"
                  v-model="form.nama_pelanggan"
                  required
                  class="w-full !p-2.5 border dark:!border-typography-3 dark:!bg-dark-primary-2 rounded-lg dark:!text-white !text-sm"
                />
              </div>

              <div>
                <label class="block mb-1 text-start dark:!text-white text-sm">Email</label>
                <input
                  type="email"
                  v-model="form.email"
                  required
                  class="w-full !p-2.5 border dark:!border-typography-3 dark:!bg-dark-primary-2 rounded-lg dark:!text-white !text-sm"
                />
              </div>

              <div>
                <label class="block mb-1 text-start dark:!text-white text-sm">Password</label>
                <input
                  type="password"
                  v-model="form.password"
                  required
                  minlength="6"
                  class="w-full !p-2.5 border dark:!border-typography-3 dark:!bg-dark-primary-2 rounded-lg dark:!text-white !text-sm"
                />
              </div>

              <div>
                <label class="block mb-1 text-start dark:!text-white text-sm">No. HP</label>
                <input
                  type="text"
                  v-model="form.no_hp"
                  placeholder="Opsional"
                  class="w-full !p-2.5 border dark:!border-typography-3 dark:!bg-dark-primary-2 rounded-lg dark:!text-white !text-sm"
                />
              </div>

              <div>
                <label class="block mb-1 text-start dark:!text-white text-sm">Alamat</label>
                <textarea
                  v-model="form.alamat"
                  required
                  class="w-full !p-2.5 border dark:!border-typography-3 dark:!bg-dark-primary-2 rounded-lg dark:!text-white !text-sm"
                ></textarea>
              </div>

              <div class="flex justify-end">
                <button
                  type="submit"
                  class="!bg-green-200 px-5 py-1 !text-sm !text-green-800 !font-medium border !border-green-800 rounded-lg"
                >
                  Simpan
                </button>
              </div>
              
            </div>
          </form>
        </div>
      </div>
    </template>
  </v-dialog>
</template>
