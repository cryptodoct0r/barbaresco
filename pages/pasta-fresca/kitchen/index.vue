<i18n>
{
  "uk": {
    "name": "Паста фреска",
    "title": "Кухня",
    "description": "Родзинкою нашого закладу є паста власного виробництва, яку готуємо з італійської муки з твердих сортів пшениці- Semola di Grano, а отже вона не зашкодить вашій фігурі. Рахуємо, що чудовий ранок починається із сніданку. Наші сніданки корисні та поживні, забезпечать енергією на увесь день, готуємо їх щодня з 10:00 по 00:30. Великий вибір страв з морепродуктів та неймовірно смачні десерти. Широкий вибір вин, смачна кава, та наше молоде італійське вино, яке нікого не залишає байдужим."

  },
  "en": {
    "name": "Pasta Fresca",
   "title": "Kitchen",
   "description": "The highlight of our establishment is a paste of our own production, which is made from Italian flour from durum wheat Semola di Grano, and therefore it will not damage your figure. We count on a great morning for breakfast. Our breakfasts are healthy and nutritious, they will provide energy for the whole day, we prepare them daily from 10:00 to 00:30. Wide selection of seafood dishes and delicious desserts. A wide selection of wines, delicious coffee, and our young Italian wine that leaves no one indifferent."

  },
  "ru": {
    "name": "Паста фреска",
    "title": "Кухня",
    "description": "Изюминкой нашего заведения есть паста собственного производства, которую готовим с итальянской муки из твердых сортов пшеници- Semola di Grano, а значит она не повредит вашей фигуре. Считаем, что чудесное утро начинается с завтрака. Наши завтраки полезны и питательны, обеспечат энергией на весь день, готовим их ежедневно с 10:00 до 00:30. Большой выбор блюд из морепродуктов и невероятно вкусные десерты. Широкий выбор вин, вкусный кофе, и наше молодое итальянское вино, которое никого не оставляет равнодушным."
  }
}
</i18n>

<template>
  <div
    v-once
    id="pasta-fresca-kitchen"
  >
    <TheTopContact
      :items="items"
      class="d-none d-md-flex w-100"
    />
    <section
      id="section2"
      class="panel overview"
      data-section-name="overview"
    >
      <div
        id="transcroller-body"
        class="block1 aos-all"
      >
        <div class="row mx-0">
          <div class="col-sm-6 p-0 col-xs-12 left-image" />

          <div class="col-sm-6 p-0 col-xs-12 left-content verticalcenter">
            <div
              class="block1-left"
              style="top: auto; transform: none;"
            >
              <div
                class="aos-item aos-init"
                data-aos="fade-left"
                data-aos-easing="ease"
                data-aos-delay="1000"
                data-aos-duration="500"
              >
                <div class="aos-item__inner">
                  <h1>{{ $t('title') }}</h1>
                  <p>
                    {{ $t('description') }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import TheTopContact from '~/components/PastaFresca/TheTopContactPastaFresca'
export default {
  name: 'PastaFrescaKitchen',
  layout: 'pasta-fresca',
  nuxtI18n: {
    paths: {
      en: '/pasta-fresca/kitchen',
      uk: '/pasta-fresca/kuhnja',
      ru: '/pasta-fresca/kuhnja'
    }
  },
  components: {
    TheTopContact
  },
  meta: {
    animation: 'overlay-down-full'
  },

  data () {
    return {
      items: [
        {
          text: this.$t('links.home'),
          to: this.localePath({ name: 'index' }, this.$i18n.locale)
        },
        {
          text: this.$t('links.menu'),
          to: this.localePath({ name: 'pasta-fresca' }, this.$i18n.locale)
        },
        {
          text: this.$t('links.kitchen'),
          active: true
        }
      ]
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
          content: (this.$t('name') + ' -' + this.$t('description')).slice(
            0,
            320
          )
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.block1 .left-image {
  background: url(~assets/img/pasta-fresca-kitchen.jpg);
}
@import "~assets/scss/subPagesStylesNew.scss";
</style>
