<template>
    <div class="page">
        <prevPage></prevPage>

        <div class="form">
            <div class="form__body">
                <h1 class="text-center">
                    Пополнение баланса
                </h1>

                <div class="type">
                    <NuxtLink to="/refill">Пополнить </NuxtLink>
                    <NuxtLink to="/withdrawal">Вывести</NuxtLink>
                </div>
                <h2>Порядок действий для вывода средств</h2>


                <div class="footer">
                    <p>1. Подтвердите Ваше согласие с правилами нашей системы</p>

                    <div>
                        <input type="radio" id="terms">
                        <label for="terms">Я согласен с <NuxtLink to="/terms"> пользовательским соглашением</NuxtLink>
                            и <NuxtLink to="/terms">политикой конфиденциальности</NuxtLink></label>
                    </div>


                    <p class="mt-4">2. Введите сумму, на которую вы хотите пополнить личный счёт, и нажмите на кнопку
                        «Пополнить». Вы будете переадресованы на сайт платёжной системы, где сможете завершить платёж.</p>

                    <div class="pay">
                        <input type="number" v-model="cost">
                        <button ref="inBtn" @click="inMoney">Пополнить</button>
                    </div>

                    <div class="select">
                        <button :class="{ active: cost == 100 }" @click="cost = 100">100 ₸</button>
                        <button :class="{ active: cost == 1000 }" @click="cost = 1000">1000 ₸</button>
                        <button :class="{ active: cost == 2000 }" @click="cost = 2000">2000 ₸</button>
                        <button :class="{ active: cost == 5000 }" @click="cost = 5000">5000 ₸</button>
                        <button :class="{ active: cost == 10000 }" @click="cost = 10000">10000 ₸</button>
                    </div>
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
            payType: '',
            cost: null,
            cardNumber: '',
            cardHolder: '',
            phone: '',
            cardNumberMaxLength: 19,
            pathUrl: 'https://rustshop.kz',

        }
    },
    methods: {
        inMoney() {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/money/new-pay`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.inBtn.innerHTML = 'Ожидайте'

            axios
                .post(path, {
                    amount: this.cost
                })
                .then(response => {
                    console.log(response)
                    window.location.href = response.data.url
                    if (response.status = 201) {
                        this.$refs.inBtn.innerHTML = 'Пополнить'
                    }
                    if (response.status == 228) {
                        this.$refs.outBtn.innerHTML = response.data.error_msg
                    }
                })
                .catch(error => {
                    console.error(error)
                    this.$refs.inBtn.innerHTML = 'Пополнить'
                })
        },
    },

}
</script>
<script setup>
useSeoMeta({
    title: 'Пополнение баланса | RustShop',
    ogTitle: 'Пополнение баланса | RustShop',
    description: 'Пополнение баланса | RustShop',
    ogDescription: 'Пополнение баланса | RustShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 73px 70px 0 200px;
    background-image: url('/assets/img/pay.png');
    background-repeat: no-repeat;
    background-position: bottom left 20%;

    @media (max-width: 1600px) {
        padding: 73px 50px 0 200px;
    }

    @media (max-width: 1024px) {
        padding: 140px 20px 0;
        background-image: none;
    }



    .form {
        z-index: 2;
        margin-top: 20px;
        padding-bottom: 140px;
        display: flex;
        justify-content: center;
        // max-width: 680px;
        position: relative;
        margin-right: 190px;

        @media (max-width: 1024px) {
            margin-right: 0;
            padding: 0;
        }

        .cardd {
            display: flex;
            flex-direction: column;

            input {
                display: block;
                border-radius: 7px;
                border: 2px solid #75382E;
                padding: 12px 10px 9px;
                background: transparent;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;

                font-family: var(--mon);
                color: #fff;
            }
        }

        .select {
            margin-top: 20px;
            display: flex;
            justify-content: center;
            gap: 11.94px;
            flex-wrap: wrap;

            .active {
                background: #75382E !important;
            }

            button {
                border-radius: 7px;
                border: 2px solid #75382E;
                background: transparent;
                padding: 11px 18px;
                transition: all .3s ease;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--mon);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }
        }

        .pay {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;

            input {
                flex: 1;
                text-align: right;
                border-radius: 7px;
                border: 2px solid #75382E;
                padding: 12px 10px 9px;
                background: transparent;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;

                font-family: var(--mon);
                color: #fff;
                max-width: 220px;

                @media (max-width: 1024px) {
                    max-width: 155px;
                }
            }

            button {
                flex: 1;
                max-width: 220px;
                padding: 13.5px 0;

                border-radius: 7px;
                background: #75382E;
                border: 0;
                box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                font-size: 15px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--unb);
                color: #fff;
                text-transform: uppercase;

                @media (max-width: 1024px) {
                    max-width: 155px;
                }
            }
        }

        .footer {
            p {
                margin: 0 0 20px;
            }

            div {
                display: flex;
                gap: 10px;

                label {
                    margin: 0;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 300;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                    max-width: 440px;

                    a {
                        color: #fff;
                        text-decoration: underline;
                    }
                }
            }
        }

        .paytype {
            p {
                margin-top: 0;
                margin-bottom: 20px;
            }

            div {
                display: flex;
                gap: 10px;
                margin-bottom: 20px;

                label {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 300;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                    margin: 0;
                }
            }
        }

        h2 {
            margin: 30px 0 20px;
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            text-align: center;
            font-family: var(--mon);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 16px;
                margin: 20px 0;
            }
        }

        p {
            max-width: 600px;
            font-size: 16px;
            font-style: normal;
            font-weight: 300;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            margin: 15px 0 0;
        }

        .type {
            margin-top: 30px;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 30px;

            a {
                text-decoration: none;
                text-align: center;
                flex: 1;
                max-width: 222px;
                padding: 10px 0;
                border-radius: 7px;
                background: #75382E;
                box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                border: 1px solid #75382E;

                font-size: 15px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 19.5px */
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 12px;
                }

                &:last-child {
                    border-radius: 7px;
                    background: #212322;
                    box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                }
            }
        }

        h1 {
            font-size: 24px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            letter-spacing: 1.6px;
            font-family: var(--unb);
            color: #fff;
            text-transform: uppercase;

            @media (max-width: 1024px) {
                font-size: 20px;
            }
        }

        .form__body {
            border-radius: 10px;
            border: 1px solid #75382E;

            background: #212322;
            padding: 30px;

            @media (max-width: 1024px) {
                padding: 20px 10px;
            }
        }
    }
}
</style>