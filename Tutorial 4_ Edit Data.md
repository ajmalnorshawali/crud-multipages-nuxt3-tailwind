# TUTORIAL 4: EDIT DATA

## STEP 1
- Create **app/pages/pengguna/kemaskini-pengguna/[id].vue**
- Copy and paste the following code
```
<script setup lang="ts"></script>

<template>
<div class="grid grid-cols-1 gap-4">
    <div class="relative w-full bg-white border border-default rounded-lg shadow-sm overflow-hidden">
        <!-- card header -->
        <div class="p-5 border-b border-default">
            <h5 class="text-lg">Kemaskini Pengguna</h5>
        </div>

        <!-- card body -->
        <div class="p-5">
            <form>
                <div class="mb-3">
                    <label for="name" class="block mb-1 font-medium">Name</label>
                    <input v-model="editForm.peopleName" type="text" id="name" class="border border-default rounded-md block w-full px-3 py-2.5 shadow-xs" />
                </div>
                <div class="mb-3">
                    <label for="email" class="block mb-1 font-medium">Email</label>
                    <input v-model="editForm.peopleEmail" type="email" id="email" class="border border-default rounded-md block w-full px-3 py-2.5 shadow-xs" />
                </div>
            </form>
            <button @click="update" type="button" class="bg-brand hover:bg-brand-strong text-white text-center font-medium rounded-md px-5 py-2.5 shadow-xs cursor-pointer">
                Kemaskini
            </button>
        </div>
    </div>
</div>
</template>
```

## STEP 2 
- Copy the following code and replace the `<script setup lang="ts"></script>`
```
<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const API_URL = 'https://leave-application.konti.cloud-connect.asia/people'

// DEFINE TYPE INTERFACE
interface PeopleForm {
    peopleName: string
    peopleEmail: string
}
const editForm = ref<PeopleForm>({ // Use the interface here
    peopleName: '',
    peopleEmail: '',
})

// READ DATA BY ID
const getDataById = async () => {
    try {
        const response = await fetch(API_URL+`/${route.params.id}`)
        const data = await response.json()

        editForm.value.peopleName = data.peopleName
        editForm.value.peopleEmail = data.peopleEmail
    } catch (error) {
        console.error('Fetch error:', error)
    }
}

// UPDATE DATA
const update = async () => {
    const { peopleName, peopleEmail } = editForm.value
    try {
        const response = await fetch(API_URL+`/${route.params.id}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                peopleName,
                peopleEmail
            }),
        })

        if (!response.ok) throw new Error('Failed to save data')

        const result = await response.json()
        console.log('Success:', result)

        // Clear form
        editForm.value.peopleName = ''
        editForm.value.peopleEmail = ''

        // Navigate to /pengguna
        router.push('/pengguna')
    } catch (error) {
        console.error('Error:', error)
    }
}

onMounted(() => {
    getDataById()
})
</script>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%203:%20Create%20Data.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%205:%20Table%20Pagination.md)
