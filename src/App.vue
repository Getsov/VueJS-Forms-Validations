<template>
  <div class="container d-flex pt-5">
    <p v-if="isLoading">Please wait, still loading...</p>
    <form class="flex-fill" @submit.prevent="handleFormSubmit">
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3"
        >
          <h1>Registration</h1>
          <hr />
          <div class="form-group">
            <label for="email">Email</label>
            <input
              type="text"
              id="email"
              class="form-control"
              v-model="$v.formData.email.$model"
            />
          </div>
          <p>Email is: {{ formData.email }}</p>

          <div class="alert alert-danger" v-if="$v.formData.email.$error">
            <p v-if="!$v.formData.email.required">
              Email is required!
            </p>
            <p v-if="!$v.formData.email.email">
              Email is invalid!
            </p>
          </div>
          <!-- <EmailInput
            :email="formData.email"
            @emailInput="formData.email = $event"
          /> -->
          <!-- <CustomEmail v-model="formData.email" /> -->
          <div class="form-group">
            <label for="password">Password</label>
            <input
              type="password"
              id="password"
              class="form-control"
              v-model="$v.formData.password.$model"
            />
            <div class="alert alert-danger" v-if="$v.formData.password.$error">
              <p v-if="!$v.formData.password.required">
                Password is required!
              </p>
              <p v-if="!$v.formData.password.minLength">
                Password must have at least
                {{ $v.formData.password.$params.minLength.min }} letters
              </p>
              <p v-if="!$v.formData.password.maxLength">
                Password must have at most
                {{ $v.formData.password.$params.maxLength.max }} letters
              </p>
              <p v-if="!$v.formData.password.alphaNum">
                Password must be alphanumeric
              </p>
            </div>

            <!---->
          </div>

          <div class="form-group">
            <label for="repeatPassword">Confirm password</label>
            <input
              type="password"
              id="repeatPassword"
              class="form-control"
              v-model="$v.formData.confirmedPassword.$model"
            />
            <div
              class="alert alert-danger"
              v-if="$v.formData.confirmedPassword.$error"
            >
              Passwords don't match
            </div>

            <!---->
          </div>

          <div class="form-group">
            <label for="age">Age</label>
            <input
              type="number"
              id="age"
              class="form-control"
              v-model="$v.formData.age.$model"
            />

            <div class="alert alert-danger" v-if="$v.formData.age.$error">
              <p v-if="!$v.formData.age.required">
                Age is required!
              </p>
              <p v-if="!$v.formData.age.minValue">
                Age must have at least
                {{ $v.formData.age.$params.minValue.min }}
              </p>
            </div>
          </div>

          <!---->
        </div>
      </div>
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 form-group"
        >
          <label for="description">Description</label> <br />
          <textarea
            id="description"
            rows="5"
            class="form-control"
            v-model="$v.formData.description.$model"
          ></textarea>
          <div class="alert alert-danger" v-if="$v.formData.description.$error">
            <p v-if="!$v.formData.description.maxLength.max">
              Description must not be more than
              {{ $v.formData.description.$params.maxLength.max }} chars
            </p>
          </div>
          <!---->
        </div>
      </div>
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3"
        >
          <div class="form-group">
            <h3>Skill Set</h3>
            <label v-for="skill in skillSets" :key="skill.id" :for="skill.id"
              ><input
                type="checkbox"
                :id="skill.id"
                :value="skill.name"
                v-model="formData.skillSet"
              />
              {{ skill.name }}
            </label>
          </div>
        </div>
      </div>
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 form-group"
        >
          <label v-for="gender in genders" :key="gender.id" :for="gender.id"
            ><input
              type="radio"
              :id="gender.id"
              :value="gender.name"
              v-model="formData.gender"
            />
            {{ gender.name }}
          </label>
        </div>
      </div>
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 from-group"
        >
          <label for="country">Country</label>
          <select id="country" class="form-control" v-model="formData.country">
            <option v-for="(country, i) in countries" :key="i">{{
              country
            }}</option>
          </select>
        </div>
      </div>
      <hr />
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3"
        >
          <button
            class="btn btn-primary"
            type="submit"
            :disabled="isSubmitDisabled"
          >
            Submit!
          </button>
        </div>
      </div>
    </form>
    <form class="flex-fill" v-if="isSubmitted">
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3"
        >
          <div class="panel panel-default">
            <div class="panel-heading">
              <h4 class="panel-heading">Your Data</h4>
            </div>
            <div class="panel-body">
              <p>Mail: {{ formData.email }}</p>
              <p>Password: {{ formData.password }}</p>
              <p>Age: {{ formData.age }}</p>
              <p>Description: {{ formData.description }}</p>
              <p>
                <strong>Skill Set?</strong>
              </p>
              <ul>
                <li v-for="skill in formData.skillSet" :key="skill">
                  {{ skill }}
                </li>
              </ul>
              <p>Gender: {{ formData.gender }}</p>
              <p>Country: {{ formData.country }}</p>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
// import EmailInput from "./components/EmailInput";
//import CustomEmail from "./components/CustomEmail";
import {
  required,
  email,
  minLength,
  maxLength,
  alphaNum,
  minValue,
  sameAs,
} from "vuelidate/lib/validators";

export default {
  name: "App",
  components: {
    // EmailInput,
    //CustomEmail,
  },
  data() {
    return {
      isLoading: false,
      skillSets: [
        {
          id: "js",
          name: "JavaScript",
        },
        {
          id: "csharp",
          name: "C#",
        },
        {
          id: "java",
          name: "Java",
        },
        {
          id: "php",
          name: "PHP",
        },
      ],
      genders: [
        {
          id: "male",
          name: "Male",
        },
        {
          id: "female",
          name: "Female",
        },
      ],
      countries: ["Bulgaria", "Germany", "England"],
      formData: {
        description: "",
        skillSet: [],
        gender: "",
        country: "",
        email: "",
        age: 0,
        password: "",
        confirmedPassword: "",
      },

      isSubmitted: false,
    };
  },
  validations: {
    formData: {
      email: { required, email },
      password: {
        required,
        minLength: minLength(4),
        maxLength: maxLength(16),
        alphaNum,
      },
      confirmedPassword: {
        sameAs: sameAs("password"),
      },
      description: { maxLength: maxLength(100) },
      age: { required, minValue: minValue(18) },
    },
  },
  computed: {
    isSubmitDisabled() {
      return this.$v.$invalid;
    },
  },
  methods: {
    async basicCall() {
      this.isLoading = true;
      try {
        await fetch("/form-submit", {
          method: "POST",
          body: JSON.stringify(this.formData),
        });
        console.log("success !");
      } catch (e) {
        console.error("Unsucess!", e);
      }

      this.isLoading = false;
    },
    async handleFormSubmit() {
      this.isSubmitted = true;
      this.$v.$touch();
      if (this.$v.$invalid) {
        await this.basicCall();
      }
    },
  },
};
</script>

<style>
@import url("https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css");
</style>
