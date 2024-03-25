<script>
//vuelidate использовал для валидации, так как очень удобный инструмент. можно очень просто кастомизировать валидации, но в тз не было требования о том как будет проходить валидация, поэтому обошелся только стилями.
import { useVuelidate } from "@vuelidate/core";
import { required, sameAs } from "@vuelidate/validators";
import Button from "./UI/Button.vue";
export default {
  components: {
    Button,
  },
  data() {
    return {
      name: "",
      phone: "",
      isPersonalAgree: false,
      step: 1,
    };
  },
  created() {
    if (localStorage.name) {
      this.name = localStorage.name;
    }
    if (localStorage.phone) {
      this.phone = localStorage.phone;
    }
  },
  validations() {
    return {
      name: { required },
      phone: { required },
      isPersonalAgree: {
        required: sameAs(true),
      },
    };
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
    submitForm() {
      this.v$.$touch();
      if (!this.v$.$invalid) {
        console.log(this.name, this.phone);
        this.step = 2;
        localStorage.removeItem('name');
        localStorage.removeItem('phone');
      }
    },
  },
  watch: {
    name(newName) {
      localStorage.name = newName;
    },
    phone(newPhone) {
      localStorage.phone = newPhone;
    }
  },
  setup() {
    return { v$: useVuelidate() };
  },
};
</script>

<template>
  <div class="backdrop" @click.self="closeModal">
    <transition name="popup" appear>
      <div class="popup">
        <button class="close-modal" @click="closeModal">
          <img src="/assets/svg/closeModal.svg" alt="X" />
        </button>
        <form v-if="step === 1" @submit.prevent="submitForm">
          <h6 class="popup__title">
            Закажите <br />
            обратный звонок
          </h6>
          <div class="popup__inputs">
            <input
              v-model.trim="name"
              class="popup__input"
              :class="{ errorInput: v$.name.$error }"
              type="text"
              placeholder="ИМЯ"
            />
            <input
              v-model.trim="phone"
              class="popup__input"
              :class="{ errorInput: v$.phone.$error }"
              type="tel"
              placeholder="ТЕЛЕФОН"
            />
          </div>
          <label class="popup__label-checkbox">
            <input v-model="isPersonalAgree" class="checkbox" type="checkbox" />
            <span
              tabindex="0"
              class="custom-checkbox"
              :class="{ errorCheckbox: v$.isPersonalAgree.$error }"
            ></span>
            <span class="popup__label-text"
              >Согласен на сохранение и обработку персональных данных</span
            >
          </label>
          <Button :styleType="2" type="submit" :isAdapt="false"
            >Заказать <span class="desctop-text">обратный</span> звонок</Button
          >
        </form>
        <div v-else-if="step === 2" class="popup__success">
          <div class="popup__success-block">
            <h6 class="popup__success-title">Спасибо за заявку</h6>
            <p class="popup__success-title">
              Я обязательно свяжусь с вами в ближайшее время.
            </p>
          </div>
          <a class="logo"><img src="/assets/svg/logo.svg" alt="" /></a>
        </div>
      </div>
    </transition>
  </div>
</template>

<style scoped lang='scss'>
.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  overflow: hidden;
  z-index: 100;
  background-color: rgba(15, 29, 69, 0.6);
  height: 100%;
  width: 100%;
}
.popup {
  width: 612px;
  height: 100%;
  background-color: rgba(15, 29, 69, 1);
  padding: 40px 32px 0 3.8%;
  position: relative;
}

.close-modal {
  position: absolute;
  right: 6%;
}

.popup__title {
  font-family: "Raleway";
  font-size: 0.5rem;
  font-weight: 600;
  line-height: 74.4px;
  margin-bottom: 13%;
}

.popup__inputs {
  display: flex;
  flex-direction: column;
  row-gap: 65px;
  margin-bottom: 21.7%;
}

.popup__input {
  width: 100%;
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.8);
  padding: 6px 2px;
  font-family: "Montserrat";
  background-color: inherit;
  font-size: 0.2rem;
  line-height: 24.56px;
  color: rgba(255, 255, 255, 0.8);
  transition: border-bottom 0.2 s ease-in-out;
}

.popup__input::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.popup__input:focus {
  outline: none;
  border-bottom: 1px solid rgba(255, 255, 255, 1);
}

.popup__label-checkbox {
  display: flex;
  align-items: center;
  margin-bottom: 17.6%;
  font-size: 0.16rem;
  line-height: 20.61px;
  letter-spacing: 0.035em;
  color: rgba(255, 255, 255, 0.4);
  column-gap: 23px;
}

.checkbox {
  display: none;
  width: 0;
  height: 0;
  appearance: none;
}

.custom-checkbox {
  min-width: 30px;
  min-height: 30px;
  border: 1px solid #fff;
  background-color: transparent;
  position: relative;
  cursor: pointer;
}
.custom-checkbox::after {
  content: "";
  display: block;
  width: 6px;
  height: 12px;
  border: solid #fff;
  border-width: 0 2px 2px 0;
  position: absolute;
  top: 50%;
  left: 50%;
  opacity: 0;
  transform: translate(-50%, -50%) rotate(45deg);
}

.checkbox:checked + .custom-checkbox::after {
  opacity: 1;
}

.popup-enter-active {
  transition: all 0.2s ease;
}
.popup-enter-from {
  transform: translateX(-100%);
}
.popup-enter-to {
  transform: translateX(0);
}

.errorInput {
  border-bottom: 1px solid red;
}

.errorCheckbox {
  border: 1px solid red;
}

.popup__success {
  display: flex;
  align-items: flex-end;
  flex-direction: column;
  height: 100%;
  justify-content: space-between;
  text-align: center;
  padding: 20px;
}
.popup__success-block {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
.popup__success-title {
  font-family: "Raleway";
  font-size: 0.5rem;
  font-weight: 600;
  line-height: 74.4px;
  margin-bottom: 66px;
}

@media (max-width: 768px) {
  .popup {
    width: 100%;
  }
  .popup__title {
    font-size: 0.2rem;
    line-height: 30px;
  }
  .popup__label-text {
    font-size: 0.11rem;
  }
  .popup__success-title {
    font-size: 0.3rem;
    line-height: 60px;
  }
}
</style>
