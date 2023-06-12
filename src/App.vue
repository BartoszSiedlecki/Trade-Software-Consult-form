<template>
  <div id="app">
  <h1>Contact us!</h1>
  <form class="form" @submit.prevent="submitForm">
      <div class="row">
        <div class="input">
          <label class="label">First name:</label>
          <input class="value" type="text" placeholder="Jan"
            v-model="formData.firstName"
            :class="{'error': hasError('firstName')}"
            :required="isRequired('firstName')"
          >
        </div>
        <div class="input">
          <label class="label">Last name:</label>
          <input class="value" type="text" placeholder="Kowalski"
            v-model="formData.lastName"
            :class="{'error': hasError('lastName')}"
            :required="isRequired('lastName')"
          >
        </div>
      </div>

      <div class="row">
        <div class="input">
          <label class="label">Email:</label>
          <input class="value" type="email" placeholder="JanKowalski@gmail.com"
            v-model="formData.email"
            :class="{'error': hasError('email')}"
            :required="isRequired('email')"
          >
        </div> 
        <div class="input">
          <label class="label">Contact number:</label>
          <input class="value" type="tel" placeholder="123-456-789"
            
            v-model="formData.phoneNumber"
            :class="{'error': hasError('phoneNumber')}"
            :required="isRequired('phoneNumber')"
          >
        </div>
      </div>

      <div class="input">
        <label class="label">Message:</label>
        <textarea  class="value message" placeholder="This is an example message"
          v-model="formData.message"
          :class="{'error': hasError('message')}"
          :required="isRequired('message')"
        ></textarea>
      </div>
      <div class="policy">
        <input class="checkbox" type="checkbox"
          v-model="formData.checkbox"
          :class="{'error': hasError('checkbox')}"
          :required="isRequired('checkbox')"
        >
        <label class="label terms">I agree to the privacy policy and terms of service.</label>
      </div>

      <div class="submit">
        <button class="value submit-btn" type="submit">Submit</button>
      </div>
  </form>
</div>
</template>

<script>
  export default{
    mounted(){
      document.title = "Contact us!";
    },
    data(){
      return{
        formData: {
          firstName: "",
          lastName: "",
          email: "",
          phoneNumber: "",
          message: "",
          checkbox: false,
        },
        formRules:{
          firstName: { required: false, maxLength: 32 },
          lastName: { required: false, maxLength: 48 },
          email: { required: true, regex: /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/ },
          phoneNumber: { required: false, regex: /^[0-9]{6,15}$/ },
          message: { required: false, maxlength: 250 },
          checkbox: { required: true }
        },
        formErrors:{ }
      }
    },
    methods:{
      submitForm(){
        if (this.formValidation()) {
          console.log("Form sent!");
        } else {
          console.log(this.formErrors);
        }
      },
      formValidation(){
        this.formErrors = {};

        for(const i in this.formRules){
          const rules = this.formRules[i];
          const value = this.formData[i];

          if (rules.required){
            if (!value){
              this.formErrors[i] = "This field is required";
            } else{
              if (rules.maxLength && value.length > rules.maxLength){
                this.formErrors[i] = `Maximum length exceeded (${rules.maxLength})`;
              } else if (rules.regex && !rules.regex.test(value)){
                this.formErrors[i] = "This field has an invalid value";
              }
            }
          } else if (value){
            if (rules.maxLength && value.length > rules.maxLength){
              this.formErrors[i] = `Maximum length exceeded (${rules.maxLength})`;
            } else if (rules.regex && !rules.regex.test(value)){
              this.formErrors[i] = "This field has an invalid value";
            }
          }
        }

        return Object.keys(this.formErrors).length === 0;
      },
      hasError(field) {
        return field in this.formErrors;
      },
      isRequired(field) {
        const rules = this.formRules[field];
        return rules && rules.required;
      },
    }
  }
</script>

<style>
  body{
    background-color: rgb(40,40,40);
    color: white;
    display: grid;
    place-items: center;
    height: 85vh;
    font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  }

  #app{
    display: grid;
    place-items: center;
  }

  h1{
    color: orange;
  }

  .form{
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
  }

  .row{
    display: flex;
    justify-content: space-between;
    gap: 4rem;
  }

  .input{
    display: flex;
    flex-direction: column;
    gap: .2rem;
    width: 100%;
  }

  .label{
    font-size: 1.5rem;
    color: rgba(255, 255, 255, 0.8);
  }

  .value{
    height: 2rem;
    font-size: 1.5rem;
    border: orange 3px solid;
    border-radius: 2px;
    background-color: rgb(40,40,40);
    color: white;
    padding: .2rem;
    transition: .1s ease-in-out all;
  }

  .value:focus{
    border: orange 3px solid;
    transform: scale(102%);
    outline: none;
  }

  .message{
    resize: none;
    overflow-y: scroll;
    width: 100%;
    height: 6rem;
    font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  }

  ::-webkit-scrollbar {
    width: .4rem;
  }

  ::-webkit-scrollbar-track {
      background: rgb(40,40,40);
  }
    
  ::-webkit-scrollbar-thumb {
      background: orange;
  }

  .policy{
    width: 100%;
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .checkbox{
    height: 1.5rem;
    width: 1.5rem;
    margin: 0;
    appearance: none;
    background-color: rgb(40,40,40);
    color: orange;
    border: .2rem solid orange;
    border-radius: 2px;
    display: grid;
    place-content: center;
    cursor: pointer;
  }

  .checkbox::before{
    content: "";
    width: .75rem;
    height: .75rem;
    background-color: orange;
    transform: scale(0);
    transition: .1s ease-in-out;
  }

  .checkbox:checked::before{
    transform: scale(1);
  }

  .submit{
    display: flex;
    justify-content: center;
    width: 100%;
  }

  .submit-btn{
    height: 3rem;
    width: 10rem;
    cursor: pointer;
  }

  .submit-btn:hover{
    background-color: orange;
    color: rgb(40,40,40);
  }

  .error{
    border: .2rem solid red;
  }
</style>
