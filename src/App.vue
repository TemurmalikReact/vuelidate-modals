<template>
  <div class="app">
    <!-- Content -->
    <section class="section">
      <div class="container">
        <button @click="firstModal.open = true">First Modal</button>
        <button @click="secondModal.open = true">Second Modal</button>
        <button @click="validation.open = true">Validation Modal</button>
        <Modal
          v-show="firstModal.open"
          title="First Modal"
          @close-modal="firstModal.open = false"
        >
          <div class="modal-content">
            <p>
              Lorem, ipsum dolor sit amet consectetur adipisicing elit. Eum
              deserunt totam facilis ex autem rerum?
            </p>
            <button @close-modal="firstModal.open = false">Submit</button>
          </div>
        </Modal>
        <Modal
          v-show="secondModal.open"
          title="Second Modal"
          @close-modal="secondModal.open = false"
        >
          <form @submit.prevent="submitSecondModal" class="second-modal__form">
            <label>
              <span>Name</span>
              <input required v-model="secondModal.name" type="text" />
            </label>
            <label>
              <span>Email</span>
              <input required v-model="secondModal.email" type="text" />
            </label>
            <button>Submit</button>
          </form>
        </Modal>

        <!-- Validation -->
        <Modal
          v-show="validation.open"
          title="Valition Modal"
          @close-modal="validation.open = false"
        >
          <form
            @submit.prevent="submitValidation"
            class="validation-modal__form"
          >
            <label :class="{ error: $v.validation.name.$error }">
              <p 
                class="error-message"
                v-if="!$v.validation.name.required"
              >
                Field is required!
              </p>
              <p
                class="error-message"
                v-if="!$v.validation.name.minLength"
              >
                Field Should be at Least
                {{ $v.validation.name.$params.minLength.min }} characters!
              </p>
              <span>Name</span>
              <input
                :class="{ error: $v.validation.name.$error }"
                @change="$v.validation.name.$touch"
                v-model="validation.name"
                type="text"
              />
            </label>
            <label :class="{ error: $v.validation.email.$error }">
              <p
                class="error-message"
                v-if="!$v.validation.email.required"
              >
                Field is required!
              </p>
              <p
                class="error-message"
                v-if="!$v.validation.email.email"
              >
                Email Is Incorrect!
              </p>
              <span>Email</span>
              <input
                :class="{ error: $v.validation.email.$error }"
                @change="$v.validation.email.$touch"
                v-model="validation.email"
                type="text"
              />
            </label>
            <button>Submit</button>
          </form>
        </Modal>
      </div>
    </section>

    <!-- Footer -->
  </div>
</template>

<script>
import Modal from "@/components/Modal.vue";
import { required, minLength, email } from "vuelidate/lib/validators";

export default {
  name: "App",
  components: {
    Modal,
  },
  data() {
    return {
      firstModal: {
        open: false,
      },
      secondModal: {
        open: false,
        name: "",
        email: "",
      },
      validation: {
        open: true,
        name: "",
        email: "",
      },
    };
  },
  validations: {
    validation: {
      name: {
        required,
        minLength: minLength(5),
      },
      email: {
        required,
        email,
      },
    },
  },
  methods: {
    submitSecondModal() {
      this.secondModal.name = "";
      this.secondModal.email = "";
      this.secondModal.open = false;
    },
    submitValidation() {
      this.$v.$touch()
      
      if(!this.$v.$invalid) {
        console.log(this.validation)

        this.validation.open = false;
        this.validation.name = "";
        this.validation.email = "";

        this.$v.$reset()
      }
    },
  },
};
</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css2?family=DM+Sans&family=Montserrat&family=Merriweather+Sans&family=Poppins:wght@600;700&display=swap);
body {
  overflow-y: scroll;
}

* {
  font-family: "Montserrat";
  font-weight: 600;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

button,
svg {
  cursor: pointer;
  outline: none;
}
</style>

<style lang="scss" scoped>
.section {
  .container {
    width: 65%;
    margin: 25px auto;
    display: flex;
    justify-content: center;
    gap: 25px;

    button {
      width: fit-content;
      padding: 10px 25px;
      font-family: "Montserrat";
      border-radius: 30px;
      border: none;
      color: #fff;
      font-weight: 600;
      cursor: pointer;
      background: rgb(50, 50, 225);

      &:hover {
        background: rgb(75, 75, 250);
      }
    }

    .second-modal__form {
      display: flex;
      flex-direction: column;

      label {
        display: flex;
        flex-direction: column;

        input {
          border: 1px solid rgb(245, 245, 245);
          outline: none;
          padding: 7.5px 15px;
        }
      }
    }

    .validation-modal__form {
      display: flex;
      flex-direction: column;
      height: 250px;
      width: 350px;

      label {
        display: flex;
        flex-direction: column;
        margin-top: 25px;

        span {
          font-size: 15px;
          margin-bottom: 5px;
        }

        input {
          border: 1px solid rgb(245, 245, 245);
          padding: 7px 14px;
          outline: none;
        }

        .error-message {
          text-align: center;
          display: none;
          font-size: 11px;
        }

        &.error {
          color: rgb(225, 0, 25);

          .error-message {
            display: block;
          }

          span {
            color: rgb(225, 0, 25);
          }

          input {
            color: rgb(225, 0, 25);
          }
        }
      }

      button {
        margin-top: auto;
      }
    }
  }
}
</style>
