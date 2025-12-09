# TUTORIAL 7: CONFIRMATION MESSAGE

## STEP 1
- Open **app/pages/pengguna/tambah-pengguna.vue**
- Copy the following code and replace `const save = async () => {...}`
```
const isAlert = ref(false)

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
        isAlert.value = true

        // Clear form
        createForm.value.peopleName = ''
        createForm.value.peopleEmail = ''

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

- Copy and paste the following code into `<template>...</template>`
```
<div v-if="isAlert" class="p-4 mb-4 text-sm text-fg-success-strong rounded-base bg-success-soft" role="alert">
    <span class="font-medium">Berjaya!</span> Data berjaya disimpan.
</div>
```

## STEP 2
- Open **app/pages/pengguna/kemaskini-pengguna/[id].vue**
- Copy the following code and replace `const update = async () => {...}`
```
const isAlert = ref(false)

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
        isAlert.value = true

        // Clear form
        editForm.value.peopleName = ''
        editForm.value.peopleEmail = ''

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

- Copy and paste the following code into `<template>...</template>`
```
<div v-if="isAlert" class="p-4 mb-4 text-sm text-fg-success-strong rounded-base bg-success-soft" role="alert">
    <span class="font-medium">Berjaya!</span> Data berjaya dikemaskini.
</div>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-tailwind/blob/main/Tutorial%206_%20Table%20Filtering.md)
|
[Next >>](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-tailwind/blob/main/Tutorial%208_%20Form%20Validation.md)
