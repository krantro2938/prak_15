<template>
  <div class="min-vh-100 py-5" style="background-color: #fafafa;">
    <div class="container" style="max-width: 800px;">
      
      <!-- Header -->
      <div class="text-center mb-5">
        <h1 class="fw-light" style="font-size: 2rem; color: #333;">Моя коллекция книг</h1>
        <p class="text-muted">Добавьте и управляйте своей коллекцией</p>
      </div>
      
      <!-- Add Form -->
      <div class="card border-0 shadow-sm mb-5">
        <div class="card-body p-4">
          <h5 class="card-title mb-4 fw-light">Добавить книгу</h5>
          <form @submit.prevent="addBook">
            <div class="mb-3">
              <input
                v-model="newBook.title"
                type="text"
                class="form-control border-0 bg-light"
                placeholder="Название книги"
                required
              >
            </div>
            <div class="mb-3">
              <textarea
                v-model="newBook.description"
                class="form-control border-0 bg-light"
                placeholder="Описание (необязательно)"
                rows="2"
              ></textarea>
            </div>
            <div class="mb-3">
              <input
                v-model="newBook.image"
                type="url"
                class="form-control border-0 bg-light"
                placeholder="URL изображения (необязательно)"
              >
            </div>
            <button class="btn btn-dark w-100" type="submit">Добавить</button>
          </form>
        </div>
      </div>
      
      <!-- Sort & Books Grid -->
      <div class="d-flex justify-content-between align-items-center mb-4">
        <span class="text-muted">{{ sortedBooks.length }} книг</span>
        <select v-model="sortBy" class="form-select form-select-sm border-0 bg-transparent" style="width: auto;">
          <option value="date">По дате</option>
          <option value="title">По названию</option>
        </select>
      </div>
      
      <!-- Books Grid -->
      <div class="row g-4">
        <div
          v-for="book in sortedBooks"
          :key="book.id"
          class="col-12"
        >
          <div class="card border-0 shadow-sm">
            <div class="card-body p-4">
              <div class="d-flex gap-4 align-items-start">
                <!-- Image -->
                <div 
                  v-if="book.image" 
                  class="flex-shrink-0"
                  style="width: 100px; height: 140px; overflow: hidden; border-radius: 4px;"
                >
                  <img 
                    :src="book.image" 
                    :alt="book.title"
                    class="w-100 h-100"
                    style="object-fit: cover;"
                  >
                </div>
                <div 
                  v-else 
                  class="flex-shrink-0 d-flex align-items-center justify-content-center bg-light"
                  style="width: 100px; height: 140px; border-radius: 4px;"
                >
                  <span class="text-muted small">Нет фото</span>
                </div>
                
                <!-- Content -->
                <div class="flex-grow-1">
                  <h5 class="mb-2" style="font-weight: 500;">{{ book.title }}</h5>
                  <p v-if="book.description" class="text-muted mb-2" style="font-size: 0.9rem;">{{ book.description }}</p>
                  <p class="text-muted mb-0" style="font-size: 0.8rem;">{{ formatDate(book.date) }}</p>
                </div>
                
                <!-- Delete -->
                <button
                  @click="removeBook(book.id)"
                  class="btn btn-sm btn-outline-secondary flex-shrink-0"
                >
                  ✕
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Empty State -->
      <div v-if="sortedBooks.length === 0" class="text-center py-5">
        <p class="text-muted">Коллекция пуста</p>
      </div>
      
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const books = ref([
  {
    id: 1,
    title: 'Война и мир',
    description: 'Роман-эпопея Льва Толстого',
    image: 'https://avatars.mds.yandex.net/i?id=c774b41b3fccb185dba905dbd95a2b50ca38094e-4842116-images-thumbs&n=13',
    date: new Date('2024-01-15')
  },
  {
    id: 2,
    title: 'Преступление и наказание',
    description: 'Роман Фёдора Достоевского',
    image: 'https://avatars.mds.yandex.net/i?id=79b6df7ca093730c7e5e23e5c2f00fddd4a13366-15416875-images-thumbs&n=13',
    date: new Date('2024-02-20')
  },
  {
    id: 3,
    title: 'Мастер и Маргарита',
    description: 'Роман Михаила Булгакова',
    image: 'https://avatars.mds.yandex.net/i?id=15aad93a4f168b9076fbe1f44a3c62160a162cb8-5280212-images-thumbs&n=13',
    date: new Date('2024-03-10')
  }
])

const newBook = ref({
  title: '',
  description: '',
  image: ''
})

const sortBy = ref('date')

const sortedBooks = computed(() => {
  const sorted = [...books.value]
  if (sortBy.value === 'title') {
    return sorted.sort((a, b) => a.title.localeCompare(b.title))
  } else {
    return sorted.sort((a, b) => new Date(b.date) - new Date(a.date))
  }
})

const addBook = () => {
  books.value.push({
    id: Date.now(),
    title: newBook.value.title,
    description: newBook.value.description,
    image: newBook.value.image,
    date: new Date()
  })
  newBook.value = {
    title: '',
    description: '',
    image: ''
  }
}

const removeBook = (id) => {
  books.value = books.value.filter(book => book.id !== id)
}

const formatDate = (date) => {
  return new Date(date).toLocaleDateString('ru-RU', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}
</script>

<style>
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}
</style>
