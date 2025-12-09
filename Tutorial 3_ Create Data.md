# TUTORIAL 3: CREATE DATA

## STEP 1
- Create **app/pages/pengguna/tambah-pengguna.vue**
- Copy and paste the following code
```
<script setup lang="ts"></script>

<template>
<div class="grid grid-cols-1 gap-4">
    <div class="relative w-full bg-white border border-default rounded-lg shadow-sm overflow-hidden">
        <!-- card header -->
        <div class="p-5 border-b border-default">
            <h5 class="text-lg">Tambah Pengguna</h5>
        </div>

        <!-- card body -->
        <div class="p-5">
            <form>
                <div class="mb-3">
                    <label for="name" class="block mb-1 font-medium">Name</label>
                    <input v-model="createForm.peopleName" type="text" id="name" class="border border-default rounded-md block w-full px-3 py-2.5 shadow-xs" />
                </div>
                <div class="mb-3">
                    <label for="email" class="block mb-1 font-medium">Email</label>
                    <input v-model="createForm.peopleEmail" type="email" id="email" class="border border-default rounded-md block w-full px-3 py-2.5 shadow-xs" />
                </div>
            </form>
            <button @click="save" type="button" class="bg-brand hover:bg-brand-strong text-white text-center font-medium rounded-md px-5 py-2.5 shadow-xs cursor-pointer">
                Simpan
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
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

interface PeopleForm {
    peopleName: string
    peopleEmail: string
}
const createForm = ref<PeopleForm>({ // Use the interface here
    peopleName: '',
    peopleEmail: '',
})

// CREATE DATA
const save = async () => {
    const { peopleName, peopleEmail } = createForm.value
    try {
        const response = await fetch('https://leave-application.konti.cloud-connect.asia/people', {
            method: 'POST',
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
        createForm.value.peopleName = ''
        createForm.value.peopleEmail = ''

        // Navigate to /pengguna
        router.push('/pengguna')
    } catch (error) {
        console.error('Error:', error)
    }
}
</script>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%202:%20Create%20Table.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%204:%20Edit%20Data.md)
