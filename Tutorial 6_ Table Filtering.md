# TUTORIAL 6: TABLE FILTERING

## STEP 1
- Open **app/pages/pengguna/index.vue**
- Copy the following code and paste at `<!-- filtering here -->`
```
<div class="mb-3">
    <input v-model="searchQuery" type="text" class="border border-default rounded-md block w-md px-3 py-2.5 shadow-xs" placeholder="Carian..." />
</div>
```

## STEP 2
- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
const searchQuery = ref('')
```

## STEP 3
- Copy the following code and replace `const totalPages`
```
const totalPages = computed(() => {
    const filtered = users.value.filter((user) => {
        const name = user.peopleName?.toLowerCase() || ''
        const email = user.peopleEmail?.toLowerCase() || ''
        return name.includes(searchQuery.value.toLowerCase()) ||
               email.includes(searchQuery.value.toLowerCase())
    })
    return Math.ceil(filtered.length / itemsPerPage)
})
```

## STEP 4
- Copy the following code and replace `const paginatedUsers`
```
const paginatedUsers = computed(() => {
    const filtered = users.value.filter((user) => {
        const name = user.peopleName?.toLowerCase() || ''
        const email = user.peopleEmail?.toLowerCase() || ''
        return name.includes(searchQuery.value.toLowerCase()) ||
               email.includes(searchQuery.value.toLowerCase())
    })

    const start = (currentPage.value - 1) * itemsPerPage
    const end = start + itemsPerPage
    return filtered.slice(start, end)
})
```

## STEP 5
- Save and run 
```
npm run dev
```

#
[<< Prev](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-tailwind/blob/main/Tutorial%205_%20Table%20Pagination.md)
|
[Next >>](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-tailwind/blob/main/Tutorial%207_%20Confirmation%20Message.md)
