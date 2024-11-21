<template>
    <div class="div-login">
      <div class="wrapper">
        <div class="title"><span>Login Form</span></div>
        <form action="#">
          <div class="row">
            <i class="fas fa-user"></i>
            <input v-model="email" type="text" placeholder="Email" required />
          </div>
          <div class="row">
            <i class="fas fa-lock"></i>
            <input v-model="password" type="password" placeholder="Password" required />
          </div>
          <div class="pass"><a href="#">Forgot password?</a></div>
          <div class="row button">
            <input type="button" value="Login" @click="iniciarSesion" />
          </div>
          <div class="signup-link">Not a member? <a href="#">Signup now</a></div>
        </form>
      </div>
    </div>
  </template>
  
  <style>
  /* Importing Google Fonts - Poppins */
  @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap");
  
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Poppins", sans-serif;
  }
  
  .div-login {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    padding: 15px;
    background: #1abc9c;
    overflow: hidden;
  }
  
  .wrapper {
    max-width: 500px;
    width: 100%;
    background: #fff;
    border-radius: 5px;
    box-shadow: 0px 4px 10px 1px rgba(0, 0, 0, 0.1);
  }
  
  .wrapper .title {
    height: 120px;
    background: #16a085;
    border-radius: 5px 5px 0 0;
    color: #fff;
    font-size: 30px;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .wrapper form {
    padding: 25px 35px;
  }
  
  .wrapper form .row {
    height: 60px;
    margin-top: 15px;
    position: relative;
  }
  
  .wrapper form .row input {
    height: 100%;
    width: 100%;
    outline: none;
    padding-left: 70px;
    border-radius: 5px;
    border: 1px solid lightgrey;
    font-size: 18px;
    transition: all 0.3s ease;
  }
  
  form .row input:focus {
    border-color: #16a085;
  }
  
  form .row input::placeholder {
    color: #999;
  }
  
  .wrapper form .row i {
    position: absolute;
    width: 55px;
    height: 100%;
    color: #fff;
    font-size: 22px;
    background: #16a085;
    border: 1px solid #16a085;
    border-radius: 5px 0 0 5px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .wrapper form .pass {
    margin-top: 12px;
  }
  
  .wrapper form .pass a {
    color: #16a085;
    font-size: 17px;
    text-decoration: none;
  }
  
  .wrapper form .pass a:hover {
    text-decoration: underline;
  }
  
  .wrapper form .button input {
    margin-top: 20px;
    color: #fff;
    font-size: 20px;
    font-weight: 500;
    padding-left: 0px;
    background: #16a085;
    border: 1px solid #16a085;
    cursor: pointer;
  }
  
  form .button input:hover {
    background: #12876f;
  }
  
  .wrapper form .signup-link {
    text-align: center;
    margin-top: 45px;
    font-size: 17px;
  }
  
  .wrapper form .signup-link a {
    color: #16a085;
    text-decoration: none;
  }
  
  form .signup-link a:hover {
    text-decoration: underline;
  }
  </style>
  
  <script>
  export default {
    name: "LoginForm",
    data() {
      return {
        email: "",
        password: "",
      };
    },
    methods: {
      async iniciarSesion() {
        const endpointURL = "/api/v1/user/signin";
        const user = {
          email: this.email,
          password: this.password,
        };

        try {
          //Solicitud POST a la API
          const response = await this.$api.post(endpointURL, user);

          // Verificar si la autenticación fue exitosa
          if (response.status === 200) {
            // Guardar la respuesta en el LocalStorage
            localStorage.setItem("userData", JSON.stringify(response.data));

            // Notificar éxito
            this.$q.notify({
              message: "Bienvenido",
              color: "positive",
              icon: "check_circle",
            });

            // Redirigir a la pregunta 02 (Listado de películas)
            this.$router.push("/dashboard/peliculas");
          }
        } catch (error) {
          // Manejar errores
          if (error.response && error.response.status === 401) {
            // Notificar error de credenciales
            this.$q.notify({
              message: "Credenciales incorrectas. Por favor, inténtelo de nuevo.",
              color: "negative",
              icon: "error",
            });
          } else {
            // Notificar otros errores
            this.$q.notify({
              message: "Ocurrió un error inesperado. Inténtelo más tarde.",
              color: "negative",
              icon: "error",
            });
          }
        }
      },
    },
  };
  
  </script>
  