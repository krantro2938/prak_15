<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">Моя коллекция книг</h1>
    
    <!-- Форма добавления книги -->
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <form @submit.prevent="addBook" class="card p-4 mb-4">
          <h4 class="mb-3">Добавить новую книгу</h4>
          <div class="mb-3">
            <label class="form-label">Название</label>
            <input
              v-model="newBook.title"
              type="text"
              class="form-control"
              placeholder="Введите название книги"
              required
            >
          </div>
          <div class="mb-3">
            <label class="form-label">Описание</label>
            <textarea
              v-model="newBook.description"
              class="form-control"
              placeholder="Введите описание книги"
              rows="3"
            ></textarea>
          </div>
          <div class="mb-3">
            <label class="form-label">URL изображения</label>
            <input
              v-model="newBook.image"
              type="url"
              class="form-control"
              placeholder="https://example.com/image.jpg"
            >
          </div>
          <button class="btn btn-primary" type="submit">Добавить</button>
        </form>
      </div>
    </div>
    
    <!-- Сортировка -->
    <div class="row mb-4">
      <div class="col-md-6 offset-md-3">
        <div class="d-flex align-items-center">
          <label class="form-label me-2 mb-0">Сортировать по:</label>
          <select v-model="sortBy" class="form-select" style="width: auto;">
            <option value="title">Названию</option>
            <option value="date">Дате добавления</option>
          </select>
        </div>
      </div>
    </div>
    
    <!-- Карточки книг -->
    <div class="row">
      <div
        v-for="book in sortedBooks"
        :key="book.id"
        class="col-md-4 mb-4"
      >
        <div class="card h-100">
          <img
            v-if="book.image"
            :src="book.image"
            class="card-img-top"
            :alt="book.title"
            style="height: 200px; object-fit: cover;"
          >
          <div v-else class="card-img-top bg-secondary d-flex align-items-center justify-content-center" style="height: 200px;">
            <span class="text-white">Нет изображения</span>
          </div>
          <div class="card-body">
            <h5 class="card-title">{{ book.title }}</h5>
            <p v-if="book.description" class="card-text">{{ book.description }}</p>
            <p class="card-text"><small class="text-muted">Добавлено: {{ formatDate(book.date) }}</small></p>
            <button
              @click="removeBook(book.id)"
              class="btn btn-sm btn-danger"
            >
              Удалить
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Сообщение если коллекция пуста -->
    <div v-if="sortedBooks.length === 0" class="text-center mt-5">
      <p class="text-muted">Коллекция пуста. Добавьте первую книгу!</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const books = ref([
  {
    id: 1,
    title: 'Война и мир',
    description: 'Роман-эпопея Льва Николаевича Толстого',
    image: 'https://upload.wikimedia.org/wikipedia/commons/a/a7/Voina_i_mir.jpg',
    date: new Date('2024-01-15')
  },
  {
    id: 2,
    title: 'Преступление и наказание',
    description: 'Психологический и философский роман Фёдора Достоевского',
    image: 'https://upload.wikimedia.org/wikipedia/commons/5/5c/Dostoevsky_Crime_and_Punishment.jpg',
    date: new Date('2024-02-20')
  },
  {
    id: 3,
    title: 'Мастер и Маргарита',
    description: 'Мистический роман Михаила Булгакова',
    image: 'https://upload.wikimedia.org/wikipedia/ru/1/1f/Master_i_Margarita.jpg',
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
  background-color: #f8f9fa;
}
.card {
  transition: transform 0.2s;
}
.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}
</style>
