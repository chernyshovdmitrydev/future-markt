<script>
import Modal from "./components/Modal.vue";
import Button from "./components/UI/Button.vue";
export default {
  components: {
    Modal,
    Button,
  },
  data() {
    return {
      isShowModal: false,
      GBPValue: null,
      dateValue: null,
    };
  },
  methods: {
    openModal() {
      this.isShowModal = true;
    },
    closeModal() {
      this.isShowModal = false;
    },
    async fetchValuteValue(valuteArg) {
      try {
        const response = await fetch('https://www.cbr-xml-daily.ru/daily_json.js')
        const data = await response.json()
        const value = Math.round(data.Valute[valuteArg].Value)
        return value
      } catch (e) {
        console.log(e)
      }
    },
    dateComputedReduced() {
      const today = new Date();
      const value = today.toLocaleDateString().replace(/\D/g, '').split('')
      const valueReduced = value.reduce((acc,digit) => acc + parseInt(digit), 0)
      return valueReduced
    }
  },
  created() {
      this.fetchValuteValue('GBP').then(value => {
        this.GBPValue = value
      })
      this.dateValue = this.dateComputedReduced();
    },
};
</script>

<template>
  <header>
    <div class="container">
      <nav class="header__nav">
        <a class="logo"><img src="/assets/svg/logo.svg" alt="" /></a>
        <ul class="header__menu">
          <li><a href="#">Обо мне</a></li>
          <li><a href="#">Наставничество</a></li>
          <li><a href="#">Мероприятия</a></li>
          <li><a href="#">Кейсы</a></li>
          <li><a href="#">Отзывы</a></li>
          <li><a href="#">Контакты</a></li>
        </ul>
        <div class="header__mini-menu">
          <a class="open-menu">
            <button>
              <img src="/assets/svg/mobileNavButton.svg" alt="phone" />
            </button>
          </a>
          <a class="call" href="tel:83451233445">
            <img src="/assets/svg/phone-button.svg" alt="phone" />
            <span href="tel:83451233445" class="phone">8-345-123-34-45</span>
          </a>
        </div>
      </nav>
    </div>
  </header>
  <main>
    <div class="container">
      <div class="content">
        <div>
          <h1 class="main__title">СОЗДАЮ УСЛОВИЯ ДЛЯ&nbsp;ВАШЕГО УСПЕХА</h1>
          <p class="main__desc">
            <span class="desctop-text"
              >Когда ваше время и энергия лучше сфокусированы, стремление к
              новым возможностям становится реальностью, &nbsp;</span
            >Ваш успех зависит от ваших действий
          </p>
          <div class="main__links">
            <Button :styleType="1" @click="openModal"
              >Записаться
              <span class="desctop-text">на консультацию</span></Button
            >
            <Button :styleType="2" @click="openModal"
              ><span class="desctop-text">Бесплатная консультация</span>
              <span class="mobile-text">Закать звонок</span></Button
            >
          </div>
          <div class="main__stats">
            <div class="main__stat">
              <span class="main__stat-number">{{dateValue}}+</span>
              <span class="main__stat-text"
                ><span class="desctop-text">техник для достижения целей</span>
                <span class="mobile-text">техники</span></span
              >
            </div>
            <div class="main__stat">
              <span class="main__stat-number">{{GBPValue}}%</span>
              <span class="main__stat-text"
                ><span class="desctop-text">увеличение личной</span> продуктивности</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
  <Modal v-if="isShowModal" @closeModal="closeModal" />
</template>

<style scoped lang="scss">
.container {
  max-width: 1400px;
  padding: 0 40px;
  margin: 0 auto;
}

.header__nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 39px;
}

.header__menu {
  display: flex;
  gap: 30px;
  font-size: 0.14rem;
  line-height: 17px;
}

.header__mini-menu {
  display: flex;
  align-items: center;
  gap: 15px;
}

.call {
  display: flex;
  align-items: center;
  gap: 15px;
  font-weight: 600;
  font-size: 0.18rem;
  letter-spacing: 0.02em;
}

.open-menu {
  display: none;
}

.content {
  display: grid;
  grid-template-columns: 64.4% 1fr;
}

.main__title {
  font-family: "Raleway";
  font-weight: 600;
  font-size: 0.65rem;
  line-height: 87px;
}

.main__desc {
  font-family: "Raleway";
  border-left: 1px solid rgba(255, 255, 255, 1);
  font-size: 0.16rem;
  line-height: 24px;
  color: rgba(255, 255, 255, 0.5);
  padding-left: 20px;
  max-width: 699px;
  font-weight: 400;
  margin-bottom: 7.5%;
}

.main__links {
  display: grid;
  grid-template-columns: 49.4% 52.9%;
  margin-bottom: 14%;
  grid-auto-flow: dense;
}

.main__stats {
  display: grid;
  grid-template-columns: 49.4% 52.9%;
}

.main__stat {
  display: flex;
  flex-direction: column;
  max-width: 190px;
  border-left: 1px solid rgba(255, 255, 255, 0.61);
  margin-bottom: 50px;

  &:first-child {
    padding-left: 24px;

    .main__stat-number {
      margin-bottom: 26px;
    }
  }

  &:last-child {
    padding-left: 29px;

    .main__stat-number {
      margin-bottom: 30px;
    }
  }
}

.main__stat-number {
  font-family: Montserrat;
  font-size: 0.4rem;
  font-weight: 600;
  line-height: 56.52px;
  text-align: left;
  color: rgba(255, 255, 255, 1);
}

.main__stat-text {
  font-family: "Raleway";
  font-size: 0.16rem;
  font-weight: 400;
  line-height: 19.65px;
  text-align: left;
  color: rgba(255, 255, 255, 0.5);
}

.header__menu li,
a {
  transition: all 0.3s ease;

  &:hover {
    transform: scale(110%);
  }
}

@media (max-width: 1440px) {
  .open-menu {
    display: block;
  }

  .header__menu {
    display: none;
  }

  .content {
    grid-template-columns: 80% 1fr;
  }

  .main__links {
    grid-template-columns: 100%;
    gap: 20px;
  }

  .phone {
    display: none;
  }
}

@media (max-width: 1024px) {
  .main__title {
    font-size: 0.5rem;
    line-height: 60px;
  }
}

@media (max-width: 768px) {
  .content {
    grid-template-columns: 1fr;
  }

  .main__title {
    font-size: 0.45rem;
    line-height: 60px;
  }
}

@media (max-width: 460px) {
  .main__title {
    font-size: 0.25rem;
    line-height: 33px;
    margin-bottom: 35px;
  }

  .main__desc {
    font-size: 0.11rem;
  }

  .main__stat {
    display: flex;
    flex-direction: column;
    max-width: 190px;
    border-left: 1px solid rgba(255, 255, 255, 0.61);
    margin-bottom: 50px;

    &:first-child {
      padding-left: 8px;

      .main__stat-number {
        margin-bottom: 2px;
      }
    }

    &:last-child {
      padding-left: 8px;

      .main__stat-number {
        margin-bottom: 2px;
      }
    }
  }

  .main__stat-number {
    font-size: 0.16rem;
  }

  .main__stat-text {
    font-size: 0.14rem;
  }
}
</style>

