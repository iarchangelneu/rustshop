<template>
    <header v-if="!hideHeaderOnPages.includes($route.name)">
        <div class="header__body">
            <div class="sidebar">
                <div class="sidebar__body" :class="{ navActive: sideActive }">
                    <div class="link">
                        <NuxtLink to="/">
                            <img src="@/assets/img/headerlogo.svg" alt="">
                        </NuxtLink>
                        <NuxtLink to="/" style="text-decoration: none;">
                            <h1 :class="{ inactive: !sideActive }">RustShop</h1>
                        </NuxtLink>
                        <img src="@/assets/img/burger2.svg" class="closeh" @click="sideActive = !sideActive" alt="">
                    </div>
                    <NuxtLink to="/" class="navik">
                        <img src="@/assets/img/h1.svg" :class="{ activeimg: $route.path === '/' }" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/' }">Главная</span>
                    </NuxtLink>
                    <a href="/#faq" class="navik">
                        <img :class="{ activeimg: $route.path === '/#faq' }" src="@/assets/img/h2.svg"
                            style="min-width: 50px; height: 50px;" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/#faq' }">FAQ</span>
                    </a>
                    <NuxtLink to="/catalog" class="navik">
                        <img :class="{ activeimg: $route.path === '/catalog' }" src="@/assets/img/h3.svg" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/catalog' }">Магазин</span>
                    </NuxtLink>
                    <NuxtLink to="/sale" class="navik">
                        <img :class="{ activeimg: $route.path === '/sale' }" src="@/assets/img/h4.svg" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/sale' }">продажа</span>
                    </NuxtLink>
                </div>
                <img src="@/assets/img/burger.svg" v-if="!sideActive" class="open" @click="sideActive = !sideActive" alt="">
            </div>
            <div class="acc__info">
                <div class="acc__body" v-if="isAuth">
                    <NuxtLink to="/withdrawal" class="cash">
                        <img src="@/assets/img/cash.svg" alt="">
                        <span v-if="balance !== null">{{ balance == null ? '0 ₸' :
                            balance.toFixed(1).toLocaleString()
                            + ' ₸' }}</span>
                    </NuxtLink>

                    <img src="@/assets/img/cart.svg" style="cursor: pointer;" @click.stop="toggleCart" alt="">

                    <NuxtLink to="/account" class="user">
                        <img src="@/assets/img/avatar.svg" alt="">
                        <span>личный кабинет</span>
                    </NuxtLink>
                </div>
                <div class="acc__body" v-if="!isAuth">
                    <NuxtLink to="/register" class="user">
                        <span>Вход/Регистрация</span>
                    </NuxtLink>
                </div>
            </div>
        </div>
        <div class="headermob">
            <NuxtLink to="/">
                <img src="@/assets/img/headermob.svg" class="img-fluid" alt="">
            </NuxtLink>
            <div class="yas">
                <img src="@/assets/img/cart.svg" v-if="isAuth" style="cursor: pointer;" @click.stop="toggleCart" alt="">
                <NuxtLink to="/account" class="user" v-if="isAuth">
                    <img src="@/assets/img/avatar.svg" alt="">
                </NuxtLink>
                <div class="burg">
                    <input id="menu__toggle" class="d-none" type="checkbox" />
                    <label class="menu__btn" for="menu__toggle" @click="menuOpen = !menuOpen">
                        <span></span>
                    </label>
                </div>
            </div>

            <div class="mobmenu" :class="{ activeMenu: menuOpen }">
                <div>
                    <NuxtLink to="/withdrawal" class="cashik" v-if="isAuth">
                        <img src="@/assets/img/cash.svg" alt="">
                        <span v-if="balance !== null">{{ balance == null ? '0 ₸' :
                            balance.toFixed(1).toLocaleString()
                            + ' ₸' }}</span>
                    </NuxtLink>
                    <NuxtLink to="/register" class="user cashik" v-if="!isAuth">
                        <span>Вход/Регистрация</span>
                    </NuxtLink>

                    <NuxtLink to="/" class="navik">
                        <img src="@/assets/img/h1.svg" :class="{ activeimg: $route.path === '/' }" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/' }">Главная</span>
                    </NuxtLink>
                    <a href="/#faq" class="navik">
                        <img :class="{ activeimg: $route.path === '/#faq' }" src="@/assets/img/h2.svg"
                            style="min-width: 50px; height: 50px;" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/#faq' }">FAQ</span>
                    </a>
                    <NuxtLink to="/catalog" class="navik">
                        <img :class="{ activeimg: $route.path === '/catalog' }" src="@/assets/img/h3.svg" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/catalog' }">Магазин</span>
                    </NuxtLink>
                    <NuxtLink to="/sale" class="navik">
                        <img :class="{ activeimg: $route.path === '/sale' }" src="@/assets/img/h4.svg" alt="">
                        <span :class="{ inactive: !sideActive, activeSpan: $route.path === '/sale' }">продажа</span>
                    </NuxtLink>
                </div>
            </div>
        </div>
    </header>
    <div class="cart" :class="{ cartOpen: cartOpen }" @click="stopPropagation">
        <div class="cart__header">
            <h2>Корзина</h2>
            <span @click.stop="toggleCart">Закрыть</span>
        </div>
        <div class="cart__body">
            <div class="cart__item" v-for="item in cart" :key="item.id">
                <img class="main__img" :src="item.item.img" alt="">
                <div class="item__info">
                    <div>
                        <span>{{ item.item.name }}</span>
                        <small>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</small>
                    </div>
                    <img src="@/assets/img/delete.svg" @click="deleteItem(item.id)" alt="">
                </div>
            </div>
        </div>
        <div class="cart__footer">
            <div class="total">
                <h2>итого:</h2>
                <h2>{{ totalAmount }} ₸</h2>
            </div>
            <NuxtLink to="/cart">оформить заказ</NuxtLink>
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
            sideActive: false,
            cartOpen: false,
            hideHeaderOnPages: ['login', 'register'],
            menuOpen: false,
            pathUrl: 'https://rustshop.kz',
            cart: [],
            balance: null,
            isAuth: false,
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
            this.$refs.purchase.innerHTML = 'Покупаем'

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getCart()
                    this.$refs.purchase.innerHTML = 'Куплено'
                })
                .catch(error => {
                    console.error(error);
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
        toggleCart() {
            this.cartOpen = !this.cartOpen;
            if (this.cartOpen) {
                document.addEventListener('click', this.closeCart);
                this.getCart()
            } else {
                document.removeEventListener('click', this.closeCart);
            }
        },
        closeCart() {
            this.cartOpen = false;
            document.removeEventListener('click', this.closeCart);
        },
        stopPropagation(event) {
            event.stopPropagation();
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getCart()
            this.isAuth = true
        }
        else {
            this.isAuth = false
        }
    }
}
</script>
<style lang="scss" scoped>
.cartOpen {
    right: 0 !important;
}

.cart {
    width: 600px;
    height: 840px;
    margin-top: 100px;
    position: fixed;
    right: -3000px;
    z-index: 250;
    border-radius: 10px 0px 0px 10px;
    border: 1px solid #75382E;
    background: #212322;
    transition: all .3s ease;
    padding: 30px 70px 44px 25px;

    @media (max-width: 1024px) {
        width: 100%;
        margin-top: 140px;
        border-radius: 10px;
        padding: 20px 15px 31px 20px;
    }

    .cart__footer {
        margin-top: 30px;

        a {
            display: inline-block;
            margin-top: 30px;
            border-radius: 7px;
            background: #75382E;
            box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
            border: 0;
            padding: 10px 34px;

            font-size: 15px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 19.5px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
            text-decoration: none;
        }

        .total {
            display: flex;
            justify-content: space-between;

            padding: 0 40px 0 0;

            h2 {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 26px */
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;
                margin: 0;
            }
        }
    }

    .cart__body {
        /* плашка-бегунок и фон */
        scrollbar-color: #999 #333;
    }

    .cart__body::-webkit-scrollbar {
        width: 10px;
        height: 10px;
    }

    .cart__body::-webkit-scrollbar-thumb {
        /* плашка-бегунок */
        border-radius: 10px;
        background: #75382E;
    }

    .cart__body::-webkit-scrollbar-track {
        /* фон */
        border-radius: 10px;
        background: #000;
    }

    .cart__body {
        margin-top: 40px;
        height: 580px;
        overflow-x: auto;
        display: flex;
        gap: 20px;
        padding-right: 20px;
        flex-direction: column;

        @media (max-width: 1024px) {
            padding: 0;
        }

        .cart__item {
            display: flex;
            gap: 20px;

            .item__info {
                width: 100%;
                display: flex;
                justify-content: space-between;
                align-items: flex-start;

                div {
                    display: flex;
                    flex-direction: column;
                    gap: 20px;

                    span {
                        font-size: 15px;
                        font-style: normal;
                        font-weight: 300;
                        line-height: 130%;
                        font-family: var(--unb);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 12px;
                        }
                    }

                    small {
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

                img {
                    cursor: pointer;
                }
            }

            .main__img {
                width: 149px;
                height: 99px;
                object-fit: cover;

                @media (max-width: 1024px) {
                    width: 128px;
                    height: 86px;
                }
            }
        }
    }

    .cart__header {
        display: flex;
        justify-content: space-between;
        align-items: center;

        h2 {
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 26px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
            margin: 0;
        }

        span {
            font-size: 15px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 19.5px */
            text-decoration-line: underline;
            font-family: var(--mon);
            color: #fff;
        }
    }
}

header {
    position: fixed;
    width: 100%;
    z-index: 150;

    .headermob {
        display: none;

        .activeMenu {
            right: 0 !important;
        }

        .mobmenu {
            position: fixed;
            width: 100vw;
            right: -3000px;
            transition: all .3s ease;
            top: 0;
            height: 100vh;
            background: #111110;
            display: flex;
            flex-direction: column;
            // justify-content: center;
            align-items: center;

            .navik {
                text-decoration: none;
                display: flex;
                width: 100%;
                margin-bottom: 20px;

                .activeimg {
                    border-radius: 10px;
                    background: rgba(217, 217, 217, 0.30);

                }

                .activeSpan {
                    border-radius: 10px;
                    background: rgba(255, 255, 255, 0.20);
                    margin-left: -49px;
                    padding: 16px 38px 14px 70px;
                    width: 100%;
                }

                img {
                    padding: 13px;
                    transition: all .3s ease;
                }

                span {
                    padding: 16px 38px 14px 20px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    text-transform: uppercase;
                    transition: all .3s ease;

                    font-family: var(--mon);
                    color: #fff;
                }
            }

            .cashik {
                margin-bottom: 99px;
                margin-top: 171px;
                display: flex;
                gap: 5px;

                span {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                }
            }
        }

        @media (max-width: 1024px) {
            position: relative;
            display: flex;
            justify-content: space-between;
            gap: 25px;
            align-items: center;
            padding: 40px 20px 20px;
            border-radius: 0 0 20px 20px;
            background: rgba(117, 56, 46, 0.50);

            backdrop-filter: blur(2px);
            width: 100%;
        }

        .yas {
            display: flex;
            gap: 25px;
            align-items: center;
        }
    }

    .header__body {
        position: relative;
        display: flex;
        justify-content: space-between;
        align-items: flex-start;

        @media (max-width: 1024px) {
            display: none;
        }
    }

    .acc__info {
        position: fixed;
        right: 0;
        padding: 40px 20px 0 0;

        .acc__body {
            padding: 20px;
            border-radius: 15px;
            background: rgba(117, 56, 46, 0.50);
            backdrop-filter: blur(2px);
            display: flex;
            align-items: center;
            gap: 30px;

            .user {
                display: flex;
                gap: 20px;
                align-items: center;
                text-decoration: none;

                span {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--mon);
                    color: #fff;
                }
            }

            .cash {
                display: flex;
                gap: 5px;
                align-items: center;
                text-decoration: none;

                span {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                }
            }
        }
    }

    .sidebar {
        position: fixed;
        display: flex;
        align-items: flex-start;
        height: 100vh;
        padding: 20px 0;

        .inactive {
            opacity: 0 !important;
        }

        .navik {
            text-decoration: none;
            display: flex;
            width: 100%;

            .activeimg {
                border-radius: 10px;
                background: rgba(217, 217, 217, 0.30);

            }

            .activeSpan {
                border-radius: 10px;
                background: rgba(255, 255, 255, 0.20);
                margin-left: -49px;
                padding: 16px 38px 14px 70px;
                width: 100%;
            }

            img {
                padding: 13px;
                transition: all .3s ease;
            }

            span {
                padding: 16px 38px 14px 20px;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-transform: uppercase;
                transition: all .3s ease;

                font-family: var(--mon);
                color: #fff;
            }
        }

        .openLeft {
            left: 247px !important;
        }

        .closeh {
            margin-left: 30px;
            cursor: pointer;
        }

        .open {
            position: absolute;
            top: 73px;
            left: 120px;
            cursor: pointer;
            transition: all .3s ease;
        }

        .navActive {
            width: 294px !important;
            transition: all .3s ease;
        }

        .sidebar__body {
            border-radius: 0px 15px 15px 0px;
            background: rgba(117, 56, 46, 0.50);
            backdrop-filter: blur(2px);
            height: 100%;
            width: 100px;
            transition: all .3s ease;
            padding: 40px 30px;
            overflow: hidden;

            display: flex;
            flex-direction: column;
            gap: 34px;

            .link {
                display: flex;
                gap: 5px;
                align-items: center;

                h1 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--kha);
                    margin: 0;
                    color: #fff;
                    transition: all .3s ease;
                }
            }
        }
    }
}

#menu__toggle {
    opacity: 0;
}

.menu__btn {
    margin-top: 10px;
    display: flex;
    /* используем flex для центрирования содержимого */
    align-items: center;
    /* центрируем содержимое кнопки */
    width: 35px;
    height: 20px;
    cursor: pointer;
    z-index: 100000000;
    color: #fff;
    position: relative;
    transform: rotate(180deg);
}

.menu__btn>span {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before,
.menu__btn>span::after {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before {
    content: '';
    top: -8px;
}

.menu__btn>span::after {
    content: '';
    top: 8px;
}

#menu__toggle:checked~.menu__btn>span {
    transform: rotate(45deg);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::before {
    top: 0;
    transform: rotate(0);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::after {
    top: 0;
    transform: rotate(90deg);
    background-color: #fff;
    border-radius: 10px;
}

#menu__toggle:checked~.menu__box {
    visibility: visible;
    top: 0;
}

.menu__btn>span,
.menu__btn>span::before,
.menu__btn>span::after {
    transition-duration: .25s;
}
</style>