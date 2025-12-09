# TUTORIAL 8: FORM VALIDATION

## STEP 1
- Open terminal and run the following code
```
npm install @vuelidate/core @vuelidate/validators --save
```

## STEP 2
- Open **app/pages/pengguna/tambah-pengguna.vue**
- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

// Import Vuelidate Core + Validators
import useVuelidate from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
```

- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
// VUELIDATE RULES
const rules = computed(() => ({
    peopleName: {
        required
    },
    peopleEmail: {
        required,
        email
    },
}))
const v$ = useVuelidate(rules, createForm)
```

## STEP 3
- Copy the following code and replace `const save = async () => {...}`
```
const save = async () => {
    v$.value.$touch()
    if (v$.value.$invalid) {
        console.log('Form invalid!')
        return
    }

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
        isAlert.value = true

        // Clear form
        createForm.value.peopleName = ''
        createForm.value.peopleEmail = ''

        // Reset validation states
        v$.value.$reset()

        // Navigate to /pengguna
        setTimeout(() => {
            isAlert.value = false
            router.push('/pengguna')
        }, 3000)
    } catch (error) {
        console.error('Error:', error)
    }
}
```

## STEP 3
- Copy the following code and replace `<form>...</form>`
```
<form>
    <div class="mb-3">
        <label for="name" class="block mb-1 font-medium">Name</label>
        <input v-model="createForm.peopleName" type="text" id="name" :class="{ 'is-invalid': v$.peopleName.$error }" class="border border-default rounded-md block w-full px-3 py-2.5 shadow-xs" />
        <div v-if="v$.peopleName.$error" class="invalid-feedback">
            Ruangan ini diperlukan
        </div>
    </div>
    <div class="mb-3">
        <label for="email" class="block mb-1 font-medium">Email</label>
        <input v-model="createForm.peopleEmail" type="email" id="email" :class="{ 'is-invalid': v$.peopleEmail.$error }" class="border border-default rounded-md block w-full px-3 py-2.5 shadow-xs" />
        <div v-if="v$.peopleEmail.$error" class="invalid-feedback">
            Ruangan ini diperlukan
        </div>
        <div v-else-if="v$.peopleEmail.$errors.some(e => e.$validator === 'email')">
            Emel tidak sah
        </div>
    </div>
</form>
```

## STEP 4
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-tailwind/-/blob/main/Tutorial%207%3A%20Confirmation%20Message.md)
