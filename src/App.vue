<template>

  <div class="container" v-if="alert">
    <div class="card">
      <app-alert :message="alert">
      </app-alert>
      <app-button
          :color="'danger'"
          @action="hideAlert"
      >Скрыть</app-button>
    </div>
  </div>

  <div class="container column">

    <form class="card card-w30" @submit.prevent="create">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="content"></textarea>
      </div>

      <div class="inline">

        <app-button
            :color="'primary'"
            :content-length="contentLength"
        >Добавить
        </app-button>

        <app-button
            :content-length="isItemsEmpty"
            @action="clearItems"
        >Очистить
        </app-button>

      </div>

    </form>

    <div class="card card-w70">

      <app-button
          v-if="isItemsEmpty"
          @action="getResume"
      >Посмотреть на сервере
      </app-button>

      <app-button
          v-if="isSaved"
          :color="'danger'"
          @action="deleteResume"
      >Удалить данные с сервера
      </app-button>

      <h3 v-if="isItemsEmpty"
      >Добавьте первый блок, чтобы увидеть результат

      </h3>

      <div
          v-for="item of items"
          :key="item"
          v-else
      >
        <component
            :is="`app-${item.type}`"
            :content="item.content"
        >
        </component>
      </div>

      <app-button
          :color="'primary'"
          @action="addToDatabase"
      >
        Сохранить в базе данных
      </app-button>

    </div>

  </div>

  <div class="container">

    <app-button
      :color="'primary'"
      @action="getComments"
      v-if="isCommentsEmpty"
    >
      Загрузить комментарии
    </app-button>

    <app-button
        :color="'warning'"
        @action="hideComments"
        v-else
    >
      Скрыть комментарии
    </app-button>

    <div class="loader" v-if="isLoading"></div>

    <div class="card mt-1" v-else-if="!isCommentsEmpty">
      <h2>Комментарии</h2>
      <ul class="list" v-for="comment in comments" :key="comment.id">
        <app-comment
            :email="comment.email"
            :body="comment.body">
        </app-comment>
      </ul>
    </div>

  </div>

</template>

<script>

import AppTitle from './components/AppTitle'
import AppSubtitle from './components/AppSubtitle'
import AppAvatar from './components/AppAvatar'
import AppText from './components/AppText'
import AppButton from './components/AppButton'
import AppComment from './components/AppComment'
import AppAlert from './components/AppAlert'

export default {
  data () {
    return {
      type: 'title',
      content: '',
      items: [],
      comments: [],
      isLoading: false,
      isSaved: false,
      alert: '',
      resumeId: null,
      url: 'https://jsonplaceholder.typicode.com/comments?_limit=42'
    }
  },
  methods: {
    create() {
      if (this.isTitleCreating || this.isTitleExists) {
        this.items.push({type: this.type, content: this.content})
        this.type = 'title'
        this.content = ''
      } else {
        this.alert = 'Первым делом надо добавить заголовок!'
      }
    },
    async getComments() {
      try {
        this.isLoading = true
        const response = await fetch(this.url)
        this.comments = await response.json()
        this.isLoading = false
      } catch (error) {
        this.isLoading = false
        console.log(error)
      }
    },
    hideComments() {
      this.comments = []
    },
    clearItems() {
      this.items = []
    },
    hideAlert() {
      this.alert = ''
    },
    async addToDatabase() {
      try {
        const response = await fetch('https://vue-with-http-5cd5c-default-rtdb.europe-west1.firebasedatabase.app/resume.json', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(
              this.items
          )
        })
        const firebase = await response.json()
        this.alert = 'Данные успешно сохранены!'
        this.isSaved = true
        console.log(firebase)
      } catch (error) {
        this.alert = 'Не удалось сохранить данные!'
      }
    },
    async getResume() {
      try {
        const response = await fetch(`https://vue-with-http-5cd5c-default-rtdb.europe-west1.firebasedatabase.app/resume.json`)
        const data = await response.json()
        const keys = Object.keys(data)
        const arr = keys.map(el => {
          return data[el]
        })
        const newArr = []
        arr.forEach(el => el.forEach(el => newArr.push(el)))
        this.items = newArr
        this.isSaved = true
      } catch (error) {
        this.isSaved = false
        this.alert = 'данных на сервере не обнаружено!'
      }
    },
    async deleteResume() {
      try {
        const response = await fetch(`https://vue-with-http-5cd5c-default-rtdb.europe-west1.firebasedatabase.app/resume.json`, {
          method: 'DELETE',
          headers: {
            'Content-Type': 'application/json'
          }
        })
        const firebase = await response.json()
        this.alert = 'Данные успешно удалены!'
        this.isSaved = false
        this.items = []
        console.log(firebase)
      } catch (error) {
        this.alert = 'Не удалось удалить данные'
      }
    }
  },
  computed: {
    isItemsEmpty () {
      return this.items.length === 0
    },
    contentLength () {
      return this.content.length <= 4
    },
    isCommentsEmpty () {
      return this.comments.length === 0
    },
    isTitleExists () {
      return this.items.find(el => el.type === 'title')
    },
    isTitleCreating () {
      return this.type === 'title'
    }
  },
  components: {
    AppTitle, AppSubtitle, AppAvatar, AppText, AppButton, AppComment, AppAlert
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
