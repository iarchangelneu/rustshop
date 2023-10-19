<template>
    <div class="page">
        <prevPage></prevPage>

        <div v-if="product.length <= 0"></div>
        <div class="product" v-else>
            <div class="product__left">
                <div class="img">
                    <img :src="product.img" alt="">
                </div>

                <div class="price">
                    <span>{{ product.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                    <button ref="cart" @click="addCart">КУПИТЬ</button>
                </div>
            </div>

            <div class="product__right">
                <div class="left">
                    <h1>{{ product.name }}</h1>
                    <span>Тип: {{ product.tags.ItemType.name }}</span>
                    <span>Категория: {{ product.tags.Category.name }}</span>
                </div>
                <div class="right">
                    <h2>Описание:</h2>

                    <div v-html="product.descriptions"></div>
                </div>
            </div>
        </div>

        <div class="similar__products">
            <h2>другие предметы</h2>
            <div v-if="catalog.length <= 0"></div>
            <div class="catalog__items" v-else>
                <NuxtLink v-for="item in randomItems" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
                    <span class="disc">-{{ Math.floor(100 - (item.sell_price / item.price * 100)) }}%</span>
                    <img :src="item.img" class="main__img" alt="">

                    <h2>{{ item.name }}</h2>

                    <div class="price">
                        <small>
                            {{ item.price.toFixed(1).toLocaleString() }} ₸
                            <img src="@/assets/img/disc.svg" alt="">
                        </small>
                        <span>{{ item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                    </div>
                </NuxtLink>
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
            productId: this.$route.params.id,
            product: [],
            description: 'Жар решимости Дэвиона ненадолго сливает воедино элементы и материю',
            pathUrl: 'https://rustshop.kz',
            catalog: [],
        }
    },
    computed: {
        randomItems() {
            // Создаем копию исходного массива
            const items = [...this.catalog.results];

            // Перемешиваем массив случайным образом
            for (let i = items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [items[i], items[j]] = [items[j], items[i]];
            }

            // Возвращаем первые 6 элементов
            return items.slice(0, 6);
        }
    },
    methods: {

        getCatalog() {
            const url = `${this.pathUrl}/api/products/catalog`

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        addCart() {
            const url = `${this.pathUrl}/api/products/add-busket-item/${this.productId}`
            const token = this.getAuthorizationCookie();

            this.$refs.cart.innerHTML = 'Добавляем'
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(url)
                .then((res) => {
                    console.log(res)

                    if (res.status == 201) {
                        this.$refs.cart.innerHTML = 'Добавлен'
                    }
                    else (
                        this.$refs.cart.innerHTML = 'Ошибка'
                    )
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        getProduct() {
            const url = `${this.pathUrl}/api/products/get-detail-item/${this.productId}`
            axios
                .get(url)
                .then(response => {
                    this.product = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
    mounted() {
        this.getProduct()
        this.getCatalog()
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Страница товара | RustShop',
    ogTitle: 'Страница товара | RustShop',
    description: 'Страница товара | RustShop',
    ogDescription: 'Страница товара | RustShop',
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

    .similar__products {
        margin-top: 60px;

        @media (max-width: 1024px) {
            margin-top: 28px;
        }

        .catalog__items {
            display: grid;
            gap: 20px;
            grid-auto-flow: dense;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            margin-top: 30px;

            @media (max-width: 1024px) {
                margin-top: 20px;
                grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
            }

            .catalog__item {

                border-radius: 10px;
                background: linear-gradient(180deg, #4D4D4D 0%, #121212 100%);
                padding: 10px;
                text-decoration: none;
                position: relative;
                display: flex;
                flex-direction: column;

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
                    margin-top: auto;
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


        h2 {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 31.2px */
            letter-spacing: 1.2px;
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
            margin: 0;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }
    }

    .product {
        margin-top: 30px;
        display: flex;
        gap: 40px;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 20px;
        }

        .product__right {
            display: flex;
            gap: 30px;

            @media (max-width: 1024px) {
                flex-direction: column;
                gap: 20px;
            }

            .right {
                margin-top: 20px;

                @media (max-width: 1024px) {
                    margin-top: 0;
                }

                h2 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    margin: 0 0 20px;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                        margin: 0 0 15px;
                    }
                }

                div {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    max-width: 900px;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }

            .left {
                display: flex;
                flex-direction: column;
                gap: 20px;

                @media (max-width: 1024px) {
                    gap: 15px;
                }

                h1,
                span {
                    font-size: 24px;
                    font-style: normal;
                    color: #fff;
                    margin: 0;
                    line-height: 130%;


                }

                h1 {
                    font-family: var(--unb);
                    font-weight: 500;
                    white-space: nowrap;

                    @media (max-width: 1024px) {
                        font-size: 20px;
                    }
                }

                span {
                    font-family: var(--mon);
                    font-weight: 400;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }
        }

        .product__left {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 20px;

            .price {
                display: flex;
                flex-direction: column;
                align-items: center;
                justify-content: center;
                gap: 20px;

                @media (max-width: 1024px) {
                    flex-direction: row;
                }

                span {
                    font-size: 32px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    display: block;

                }

                button {
                    font-size: 15px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    /* 19.5px */
                    text-transform: uppercase;
                    font-family: var(--unb);
                    color: #fff;

                    padding: 10px 3.177vw;
                    border-radius: 7px;
                    background: #75382E;
                    border: 0;
                    box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);

                    @media (max-width: 1024px) {
                        padding: 10px 44px;
                    }
                }
            }

            .img {
                border-radius: 10px;
                background: linear-gradient(180deg, #4D4D4D 0%, #121212 100%);
                padding: 10px;

                img {
                    width: 100%;
                    max-width: 345px;
                    height: 229px;
                    object-fit: cover;

                    @media (max-width: 1024px) {
                        max-width: 100%;
                        height: 218px;

                    }
                }

                @media (max-width: 1024px) {
                    width: 100%;

                }
            }
        }
    }
}
</style>