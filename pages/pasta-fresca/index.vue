<i18n>
{
  "uk": {
    "name": "Паста фреска",
    "title": "Меню",
    "tooltip": "Передзвонимо за 1 хв.",
    "description": "Відкриваємо суперздібності від смаку нашої пасти Приходьте та спробуйте самі",
    "instruction": "Будь ласка, виберіть категорію продуктів зверху!",
    "offline": "Ви зараз офлайн. Перевірте підключення до Інтернету.",
    "call":{
      "name": "Ім’я",
      "phone": "Телефон",
      "send": "Зателефонуй мені!"
      }
  },
  "en": {
    "name": "Pasta Fresca",
   "title": "Menu",
   "tooltip": "Call back in 1 minute",
   "description": "Компанія друзів, смачне меню і відмінна атмосфера - тільки в Le Rock!",
   "instruction": "Please Select a Food Category Above!",
   "offline": "You are currently offline, Please check your internet connection.",
   "call":{
      "name": "Name",
      "phone": "Phone",
      "send": "Call Me!"
      }
  },
  "ru": {
    "name": "Паста фреска",
    "title": "Меню",
    "tooltip": "Перезвоним за 1 мин.",
    "description": "Затишна атмосфера, приємна музика, смачна кухня та привітний персонал - все це Ви знайдете у Нас",
    "instruction": "Пожалуйста, выберите категорию продуктов выше!",
    "offline": "Вы в настоящее время не в сети, пожалуйста, проверьте подключение к интернету.",
    "call":{
      "name": "название",
      "phone": "Телефон",
      "send": "Позвоните мне!"
      }
  }
}
</i18n>

<template>
  <div
    id="pasta-fresca"
    data-aos="fade-up"
    data-aos-easing="ease"
    data-aos-delay="250"
    data-aos-duration="500"
  >
    <TheTopContact
      :items="items"
      class="d-none d-md-flex w-100"
    />

    <TheImageNavigation />

    <TheImagedProductNav
      :current-products-displayed="currentProductsDisplayed"
      @changedView="updateView($event)"
    />

    <b-img-lazy
      id="feedBack"
      v-b-modal.modal-center
      class="feedBack pulse"
      src="~/assets/img/feedBack.png"
      alt="feedback Icon"
    />

    <div>
      <b-modal
        id="modal-center"
        centered
        hide-footer
      >
        <b-form>
          <b-form-group
            :label="$t('call.name')"
            size="lg"
            label-for="dropdown-form-name"
            @submit.stop.prevent
          >
            <b-form-input
              id="dropdown-form-name"
              v-model="$v.name.$model"
              size="sm"
              :placeholder="$t('call.name')"
              class="form-control w-100"
              type="text"
              name="name"
              :class="[!$v.name.$error && $v.name.$model && $v.name.minLength ? 'is-valid' : '', $v.name.$error && !$v.name.minLength ? 'is-invalid' : '']"
              :state="$v.name.$dirty ? !$v.name.$error : null"
            />
          </b-form-group>
          <b-form-group
            :label="$t('call.phone')"
            label-for="dropdown-form-phone"
            @submit.stop.prevent="sendContact"
          >
            <b-form-input
              id="dropdown-form-phone"
              v-model="$v.phone.$model"
              v-mask="'+38 (###) ###-####'"
              size="sm"
              placeholder="+380 ## ### ####"
              class="form-control w-100"
              type="text"
              name="phone"
              :class="[!$v.phone.$error && $v.phone.$model && $v.phone.minLength ? 'is-valid' : '', $v.phone.$error && !$v.phone.minLength ? 'is-invalid' : '']"
              :state="$v.phone.$dirty ? !$v.phone.$error : null"
            />
          </b-form-group>
          <div class="text-center">
            <b-button
              variant="outline-dark"
              size="sm"
              class="feedback-btn text-center"
              type="submit"
              :disabled="loading === true || !$v.name.minLength || !name || !$v.phone.minLength || !phone"
            >
              {{ $t('call.send') }}
            </b-button>
          </div>
        </b-form>
      </b-modal>
    </div>

    <b-tooltip
      target="feedBack"
      placement="topleft"
      variant="light"
    >
      <strong>
        <h6> {{ $t('tooltip') }}</h6>
      </strong>
    </b-tooltip>

    <vue-page-transition name="fade">
      <div
        v-if="currentProductsDisplayed === 0"
        class="container"
        style="height: 300px;"
      >
        <h2 class="text-center pt-5 text-white">
          {{ $t('instruction') }}
        </h2>
      </div>

      <div v-if="$nuxt.isOffline">
        <h3 class="text-center pt-5 ">
          {{ $t('offline') }}
        </h3>
      </div>

      <ThePasta v-if="currentProductsDisplayed === 1" />

      <TheAppetizers v-if="currentProductsDisplayed === 2" />

      <TheDesert v-if="currentProductsDisplayed === 5" />

      <TheSalad v-if="currentProductsDisplayed === 6" />

      <TheFirstCourse v-if="currentProductsDisplayed === 7" />

      <TheGarnishes v-if="currentProductsDisplayed === 8" />

      <TheDrinks v-if="currentProductsDisplayed === 9" />

      <!-- <TheAlcohol v-if="currentProductsDisplayed === 10" /> -->

      <TheMainMeal v-if="currentProductsDisplayed === 11" />
    </vue-page-transition>

    <TheBottomCarousel class="elaboraSpacing" />

    <TheMapComponent />
  </div>
</template>

<script>
import axios from 'axios'
import { required, minLength } from 'vuelidate/lib/validators'
import TheTopContact from '~/components/PastaFresca/TheTopContactPastaFresca'
import TheImageNavigation from '~/components/PastaFresca/TheImageNavigationPastaFresca'
import TheImagedProductNav from '~/components/PastaFresca/TheImagedProductNavPastaFresca'
import TheMapComponent from '~/components/PastaFresca/TheMapComponentPastaFresca'
import ThePasta from '~/components/PastaFresca/PastaFrescaProducts/ThePasta'
import TheDrinks from '~/components/PastaFresca/PastaFrescaProducts/TheDrinks'
import TheDesert from '~/components/PastaFresca/PastaFrescaProducts/TheDesert'
import TheSalad from '~/components/PastaFresca/PastaFrescaProducts/TheSalad'
import TheFirstCourse from '~/components/PastaFresca/PastaFrescaProducts/TheFirstCourse'
import TheGarnishes from '~/components/PastaFresca/PastaFrescaProducts/TheGarnishes'
import TheAppetizers from '~/components/PastaFresca/PastaFrescaProducts/TheAppetizers'
// import TheAlcohol from '~/components/PastaFresca/PastaFrescaProducts/TheAlcohol'
import TheMainMeal from '~/components/PastaFresca/PastaFrescaProducts/TheMainMeal'
import TheBottomCarousel from '~/components/PastaFresca/TheBottomCarouselOwl'

export default {
  name: 'PastaFresca',
  layout: 'pasta-fresca',
  nuxtI18n: {
    paths: {
      en: '/pasta-fresca-menu',
      uk: '/Pasta-freska-menyu',
      ru: '/Pasta-freska-menyu'
    }
  },
  components: {
    TheTopContact,
    TheImageNavigation,
    TheImagedProductNav,
    TheMapComponent,
    ThePasta,
    TheDesert,
    TheSalad,
    TheFirstCourse,
    TheGarnishes,
    TheAppetizers,
    TheDrinks,
    // TheAlcohol,
    TheMainMeal,
    TheBottomCarousel
  },

  meta: {
    animation: 'overlay-down-full'
  },

  data () {
    return {
      success: false,
      error: [],
      phone: null,
      name: null,
      items: [
        {
          text: this.$t('links.home'),
          to: this.localePath({ name: 'index' }, this.$i18n.locale)
        },
        {
          text: this.$t('links.menu'),
          active: true
        }
      ],
      currentProductsDisplayed: 0,
      loading: false

    }
  },

  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    phone: {
      required,
      minLength: minLength(18)
    }
  },

  watch: {
    name (newName) {
      localStorage.name = newName
    },
    phone (newPhone) {
      localStorage.phone = newPhone
    },
    currentProductsDisplayed (newCurrentProductsDisplayed) {
      sessionStorage.currentProductsDisplayedPF = newCurrentProductsDisplayed
    }
  },

  mounted () {
    window.addEventListener('scroll', this.onScroll)

    if (localStorage.name) {
      this.name = localStorage.name
    }

    if (localStorage.phone) {
      this.phone = localStorage.phone
    }

    if (sessionStorage.currentProductsDisplayed) {
      this.currentProductsDisplayed = parseInt(sessionStorage.currentProductsDisplayedPF) || 0
    }
  },

  created () {
    this.loading = true
    this.$store.dispatch('fetchProductsPF').then(() => (this.loading = false))
  },

  methods: {
    updateView (updatedView) {
      this.currentProductsDisplayed = updatedView
    },
    onClick () {
      this.$refs.dropdown.hide(true)
    },

    checkForm (e) {
      this.errors = []
      this.success = false

      if (!this.name) {
        this.errors.push('Ім’я вимагається')
      }
      if (!this.phone) {
        this.errors.push('Телефон вимагається')
      }
      if (!this.errors.length) {
        this.sendOrder()
      }
      e.preventDefault()
    },

    sendContact () {
      axios
        .post(`https://api.telegram.org/bot1029393497:AAH-v0VHLmNK6cURI38Ro5-Bvxb2ba73xRU/sendMessage?chat_id=-1001498927317&text= Передзвонимо за 1 хв %0AІм'я: ${this.name}, %0AНомер телефону: ${this.phone}`)
      this.success = true
      this.$bvToast.toast(`${this.$t('toast.info')}`, {
        title: `${this.$t('toast.title')}`,
        autoHideDelay: 10000,
        variant: 'success',
        toaster: 'b-toaster-top-center'
      })
    }

  },

  head () {
    return {
      title: (this.$t('name') + ' - ' + this.$t('title')).slice(
        0,
        60
      ),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: (this.$t('name') + ' - ' + this.$t('description')).slice(
            0,
            320
          )
        }
      ]
    }
  }

}
</script>

<style lang="scss">
#modal-center___BV_modal_content_ {
  // max-width: 430px;
  background: $lightColor;

  box-shadow: 0px 15px 75px rgba(0, 0, 0, 0.35);

  .modal-header {
    max-width: 330px;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    padding: 18px 0rem 30px 0rem;

    border-bottom: none;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
  }

  .close {
    font-size: 60px;
    font-weight: 300;
  }
}
</style>

<style lang="scss" scoped>
@import "~assets/scss/TheMenu.scss";

#pasta-fresca {
  background: url("~assets/img/pasta-fresca-background.jpg") no-repeat center
    center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  position: relative;
  background-color: #4c4240;
}
</style>
