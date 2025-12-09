# TUTORIAL 2: PREPARE TABLE

## STEP 1
- Create **app/pages/pengguna/index.vue**
- Copy and paste the following code
```
<script setup lang="ts"></script>

<template>
<div class="grid grid-cols-1 gap-4">
    <div class="relative w-full bg-white border border-default rounded-lg shadow-sm overflow-hidden">
        <!-- card header -->
        <div class="p-5 border-b border-default">
            <h5 class="text-lg">Senarai Pengguna</h5>
        </div>

        <!-- card body -->
        <div class="p-5">
            <div class="mb-3">
                <button @click="tambahPengguna" type="button" class="bg-brand hover:bg-brand-strong text-white text-center font-medium rounded-md px-5 py-2.5 shadow-xs cursor-pointer">
                    Tambah Pengguna
                </button>
            </div>

            <!-- filtering here -->

            <div class="relative overflow-x-auto rounded-md border border-default">
                <table class="w-full text-gray-700">
                    <thead class="uppercase bg-gray-50 rounded-md border-b border-default">
                        <tr>
                            <th scope="col" class="px-6 py-3 text-left text-xs">
                                #
                            </th>
                            <th scope="col" class="px-6 py-3 text-left text-xs">
                                Name
                            </th>
                            <th scope="col" class="px-6 py-3 text-left text-xs">
                                Email
                            </th>
                            <th scope="col" class="px-6 py-3 text-center text-xs">
                                Action
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in users" :key="item.id" class="rounded-md border-b border-default">
                            <th scope="row" class="px-6 py-4 text-left">
                                {{ index + 1 }}
                            </th>
                            <td class="px-6 py-4 text-left">
                                {{ item.peopleName }}
                            </td>
                            <td class="px-6 py-4 text-left">
                                {{ item.peopleEmail  }}
                            </td>
                            <td class="px-6 py-4 text-center">
                                <button @click="editPengguna(item.id)" class="bg-transparent text-gray-500 hover:text-gray-700 text-center font-medium cursor-pointer me-1">Edit</button>
                                <button @click="remove(item.id)" class="bg-transparent text-gray-500 hover:text-gray-700 text-center font-medium cursor-pointer">Remove</button>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <!-- pagination here -->
            </div>
        </div>
    </div>
</div>
</template>
```

## STEP 2 
- Copy the following code and replace the `<script setup lang="ts"></script>`
```
<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

// DEFINE TYPE INTERFACE
interface FeedItem {
    id: string
    peopleName: string
    peopleEmail: string
}
const API_URL = 'https://leave-application.konti.cloud-connect.asia/people'

// STATE VARIABLES
const users = ref<FeedItem[]>([])

// READ DATA
const getData = async () => {
    try {
        const response = await fetch(API_URL)
        const data = await response.json()
        users.value = data
    } catch (error) {
        console.error('Fetch error:', error)
    }
}

// DELETE DATA
const remove = async (id: number | string) => {
    try {
        const response = await fetch(API_URL+`/${id}`, {
            method: 'DELETE'
        })

        if (!response.ok) throw new Error('Failed to save data')

        console.log('Deleted successfully')

        // Refresh the list
        getData()
    } catch(error) {
        console.error('Error:', error)
    }
}

onMounted(() => {
    getData()
})

const tambahPengguna = () => {
    router.push('/pengguna/tambah-pengguna');
};

const editPengguna = (id: number | string) => {
    router.push(`/pengguna/kemaskini-pengguna/${id}`);
};
</script>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%201:%20Setup%20Nuxt3%20+%20Tailwind.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%203:%20Create%20Data.md)
