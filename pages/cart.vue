<template>
    <div class="page">
        <prevPage></prevPage>

        <h1>подтверждение покупки</h1>
        <div class="empty text-center" v-if="cart.length <= 0">
            <h2>Вы пока не добавили товары в корзину</h2>
            <NuxtLink to="/catalog">перейти в магазин</NuxtLink>
        </div>
        <div class="confirm__body" v-else>
            <div class="w-100">
                <div class="catalog__items">
                    <NuxtLink v-for="item in cart" :key="item.id" :to="'/product/' + item.item.id" class="catalog__item">
                        <span class="disc">-{{ Math.floor(100 - (item.item.sell_price / item.item.price * 100)) }}%</span>
                        <img :src="item.item.img" class="main__img" alt="">

                        <h2>{{ item.item.name }}</h2>

                        <div class="price">
                            <small>
                                {{ item.item.price.toFixed(1).toLocaleString() }} ₸
                                <img src="@/assets/img/disc.svg" alt="">
                            </small>
                            <span>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                        </div>
                    </NuxtLink>
                </div>
            </div>

            <div class="confirm__block">
                <h2>Количество товаров: {{ cart.length }}</h2>
                <h2>Итого: {{ totalAmount }} ₸</h2>
                <button ref="purchases" @click="confirmPur">подтвердить покупку</button>
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
            cart: [],
        }
    },
    computed: {
        totalAmount() {
            return this.cart.reduce((total, item) => {
                return total + item.item.sell_price;
            }, 0).toFixed(1).toLocaleString();
        },
    },

    methods: {
        confirmPur() {
            const url = `${this.pathUrl}/api/products/confirm-busket`
            this.$refs.purchases.innerHTML = 'Покупаем'

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getCart()
                    this.$refs.purchases.innerHTML = 'Куплено'
                })
                .catch(error => {
                    console.error(error.message);
                    if (error.message == 'Request failed with status code 304') {
                        this.$refs.purchases.innerHTML = 'Недостаточно средств'
                    }
                });
        },
        deleteItem(id) {
            const url = `${this.pathUrl}/api/products/delete-busket-item/${id}`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    this.getCart()
                })
        },
        getCart() {
            const url = `${this.pathUrl}/api/users/profile/`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    this.balance = res.data.balance
                    this.cart = res.data.basket
                })
        },
    },
    mounted() {
        this.getCart()
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Подтверждение покупки | RustShop',
    ogTitle: 'Подтверждение покупки | RustShop',
    description: 'Подтверждение покупки | RustShop',
    ogDescription: 'Подтверждение покупки | RustShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 73px 70px 113px 200px;

    @media (max-width: 1600px) {
        padding: 73px 50px 50px 200px;
    }

    @media (max-width: 1024px) {
        padding: 140px 20px 50px;
    }

    .empty {
        margin-top: 170px;

        @media (max-width: 1024px) {
            margin-top: 90px;
        }

        h2 {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }

        a {
            display: inline-block;
            margin-top: 50px;
            border-radius: 7px;
            background: #75382E;
            box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
            padding: 10px 20px;

            text-decoration: none;
            font-size: 15px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 19.5px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;

            @media (max-width: 1024px) {
                margin-top: 40px;
            }
        }
    }

    .confirm__body {
        display: flex;
        gap: 40px;
        align-items: flex-start;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 30px;
        }

        .confirm__block {
            min-width: 25vw;
            border-radius: 10px;
            border: 1px solid #75382E;
            background: #212322;
            padding: 42px 20px;
            text-align: center;
            display: flex;
            flex-direction: column;
            gap: 25px;

            @media (max-width: 1024px) {
                min-width: 100%;
                width: 100%;
                padding: 20px;
            }

            h2 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--unb);
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }

            button {
                border-radius: 7px;
                background: #75382E;
                box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                border: 0;
                padding: 10px 16.5px;

                font-size: 15px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 19.5px */
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;
            }
        }

        .catalog__items {
            display: grid;
            gap: 20px;
            grid-auto-flow: dense;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));

            @media (max-width: 1024px) {
                grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
            }

            .catalog__item {

                border-radius: 10px;
                background: linear-gradient(180deg, #4D4D4D 0%, #121212 100%);
                padding: 10px;
                text-decoration: none;
                position: relative;
                display: block;

                .disc {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--unb);
                    color: #fff;
                    padding: 4px 7px;
                    border-radius: 5px;
                    background: #75382E;
                    position: absolute;

                    @media (max-width: 1024px) {
                        font-size: 12px;
                    }
                }

                h2 {
                    font-size: 15px;
                    font-style: normal;
                    font-weight: 300;
                    line-height: 130%;
                    font-family: var(--unb);
                    color: #fff;
                    margin: 0 0 12px;

                    @media (max-width: 1024px) {
                        font-size: 12px;
                    }
                }

                .price {
                    display: flex;
                    gap: 6px;
                    justify-content: flex-end;

                    small {
                        position: relative;
                        font-size: 12px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 12px;
                        }

                        img {
                            position: absolute;
                            left: 0;
                            top: 0;
                        }
                    }

                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 600;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 12px;
                        }
                    }
                }

                .main__img {
                    width: 100%;
                    height: 153px;
                    object-fit: contain;
                    margin-bottom: 10px;


                    @media (max-width: 1024px) {
                        height: auto;
                    }
                }
            }
        }

    }

    h1 {
        font-size: 32px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
        margin: 30px 0;

        @media (max-width: 1024px) {
            font-size: 20px;
            text-align: center;
        }
    }
}
</style>