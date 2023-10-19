<template>
    <div class="page">
        <div class="form">
            <NuxtLink to="/" class="text-center">
                <img src="@/assets/img/reglogo.svg" class="text-center" alt="">
            </NuxtLink>

            <div class="form__body">
                <h1 class="text-center">Авторизация</h1>


                <div class="inputs">
                    <div>
                        <label for="email">Ваш e-mail</label>
                        <input type="email" name="email" id="email" v-model="email">
                    </div>

                    <div>
                        <label for="password">пароль</label>
                        <div class="showpass">
                            <input :type="showPassword ? 'text' : 'password'" name="password" id="password"
                                v-model="password">
                            <img @click="togglePasswordVisibility('password')" src="@/assets/img/eye.svg" alt="">
                        </div>
                    </div>

                    <small>{{ error }}</small>
                    <div class="buttons">
                        <button @click="login">Войти</button>
                        <button @click="loginSteam">Войти через steam</button>
                    </div>

                    <span>Нет аккаунта? <NuxtLink to='/register'>Зарегистрироваться</NuxtLink></span>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            pathUrl: 'https://rustshop.kz',
            email: '',
            password: '',
            repeat__password: '',
            showPassword: false,
            error: '',
        }
    },
    methods: {
        login() {
            const url = `${this.pathUrl}/api/users/authorization`

            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;

            axios
                .post(url, { username: this.email, password: this.password })
                .then((res) => {



                    document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                    localStorage.setItem('accountType', 'email')

                    window.location.href = '/account'


                    console.log(res)
                })
                .catch((error) => {
                    console.log(error);
                    this.error = error.response.data.non_field_errors.toString()
                });
        },
        loginSteam() {
            const url = `${this.pathUrl}/api/users/login-steam/`

            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    window.location.href = res.data.url
                })
                .catch((error) => {
                    this.error = error.response.data.detail
                    console.log(error.response);
                });
        },
        togglePasswordVisibility(inputId) {
            const inputElement = document.getElementById(inputId);
            if (inputElement) {
                this.showPassword = !this.showPassword;
                inputElement.type = this.showPassword ? 'text' : 'password';
            }
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.$router.push('/account')

        }
        else {

        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Авторизация | RustShop',
    ogTitle: 'Авторизация | RustShop',
    description: 'Авторизация | RustShop',
    ogDescription: 'Авторизация | RustShop',
})
</script>
<style lang="scss" scoped>
.page {
    height: 100vh;
    background-image: url('@/assets/img/reg.png');
    display: flex;
    justify-content: center;
    align-items: center;

    small {
        color: red;
        font-family: var(--mon);
        font-size: 14px;
    }

    .form {
        height: 100%;
        width: 550px;
        background: linear-gradient(180deg, #4D4D4D 0%, #121212 100%);
        padding: 50px 30px 50px;
        text-align: center;

        @media (max-width: 1024px) {
            padding: 50px 20px 50px;
            width: 100%;
        }

        .form__body {
            margin-top: 193px;

            .inputs {
                padding: 0 20px;
                margin-top: 30px;

                @media (max-width: 1024px) {
                    padding: 0;
                    margin-top: 20px;
                }

                span {
                    margin-top: 20px;
                    display: block;
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }

                    a {
                        text-decoration: underline;
                        color: #fff;
                    }
                }

                .buttons {
                    display: flex;
                    gap: 25px;

                    @media (max-width: 1024px) {
                        gap: 20px;
                    }

                    button {
                        padding: 10px 0;
                        flex: 1;
                        font-size: 15px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        /* 19.5px */
                        text-transform: uppercase;
                        font-family: var(--unb);
                        color: #fff;

                        border: 0;
                        border-radius: 7px;
                        background: #75382E;
                        box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);

                        @media (max-width: 1024px) {
                            font-size: 11px;
                            padding: 10px 0;
                            // flex: 1;
                        }

                        &:last-child {
                            border-radius: 7px;
                            border: 1px solid #75382E;
                            box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                            background: #212322;
                        }
                    }
                }

                .showpass {
                    position: relative;

                    img {
                        position: absolute;
                        right: 5%;
                        top: 25%;
                        cursor: pointer;
                    }
                }

                label,
                input {
                    display: block;
                }

                label {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 24px;
                    /* 150% */
                    text-transform: uppercase;
                    font-family: var(--unb);
                    color: #fff;
                    margin: 0 0 10px;
                    text-align: left;
                }

                input {
                    width: 100%;
                    border-radius: 7px;
                    border: 1px solid #452721;
                    background: #212322;
                    padding: 14px 15px;
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 24px;
                    font-family: var(--mon);
                    color: #fff;
                    margin: 0 0 30px;

                    @media (max-width: 1024px) {
                        margin: 0 0 20px;
                    }

                    &::placeholder {
                        color: #fff;
                    }
                }
            }

            small {
                display: block;
                margin: 0 0 10px;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                max-width: 491px;

                @media (max-width: 1024px) {
                    max-width: 100%;
                    font-size: 12px;
                }

                &:last-child {
                    margin: 0 0 30px;
                }
            }

            h1 {
                font-size: 32px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 41.6px */
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;
                margin: 0 0 25px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                    margin: 0 0 20px;
                }
            }
        }
    }
}
</style>