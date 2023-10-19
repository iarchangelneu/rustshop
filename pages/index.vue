<template>
    <div class="page">
        <div class="main">
            <img src="@/assets/img/mainlogo.svg" alt="" class="text-center img-fluid">

            <div class="links text-center">
                <p>Переходи в каталог, одень своих персонажей или продай свои предметы</p>

                <div class="buttons">
                    <NuxtLink to='/catalog'>Магазин</NuxtLink>
                    <NuxtLink to='/sale'>Продажа</NuxtLink>
                </div>
            </div>
            <img src="@/assets/img/xyeta.png" class="xyz" alt="">
        </div>

        <a name="sales"></a>
        <div class="sales">
            <div class="sales__body">
                <h1>Акции и скидки</h1>
                <p>Приобретайте товары по выгодным ценам</p>
                <div v-if="catalog.length <= 0"></div>
                <div class="sales__slider" v-else>
                    <swiper :slides-per-view="6" :space-between="30" :navigation="navigation" :breakpoints="breakpoints"
                        :modules="modules">
                        <swiper-slide v-for="item in randomItems" :key="item.id">
                            <NuxtLink :to="'/product/' + item.id" class="slide">
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
                        </swiper-slide>
                    </swiper>

                    <div class="navs">
                        <img src="@/assets/img/prev.svg" alt="" class="prev">
                        <img src="@/assets/img/next.svg" alt="" class="next">
                    </div>
                </div>
            </div>
        </div>
        <div class="about">
            <div class="about__body">
                <div>
                    <h1>о нас</h1>
                    <img src="@/assets/img/salesbg.png" alt="" class="blin">
                    <p>
                        Rust shop - это не просто интернет-магазин скинов и предметов для Rust. Мы — сообщество
                        страстных
                        игроков, которые разделяют вашу страсть к этой увлекательной игре. Наша цель — сделать процесс
                        приобретения скинов и оружия в Rust максимально удобным и приятным для вас.
                    </p>
                </div>

                <div>
                    <p>
                        Мы предлагаем богатый выбор скинов и оружия для Rust, помогая выделиться среди других игроков.
                        Мы
                        следим
                        за индустрией и всегда готовы предоставить самые актуальные и качественные предметы. Для нас
                        важно,
                        чтобы каждая ваша сделка на Rust Shop была надежной и приятной.
                    </p>
                </div>
            </div>

        </div>
        <div class="sales populars">
            <div class="sales__body">
                <h1>популярное</h1>
                <p>Посетите раздел, где быстро найдете самые популярные и стильные предметы, которые выбирают многие
                    игроки</p>

                <div v-if="catalog.length <= 0"></div>
                <div class="sales__slider" v-else>
                    <swiper :slides-per-view="6" :space-between="30" :navigation="navigation2" :breakpoints="breakpoints"
                        :modules="modules">
                        <swiper-slide v-for="item in randomItemsPop" :key="item.id">
                            <NuxtLink :to="'/product/' + item.id" class="slide">
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
                        </swiper-slide>
                    </swiper>

                    <div class="navs">
                        <img src="@/assets/img/prev.svg" alt="" class="prev2">
                        <img src="@/assets/img/next.svg" alt="" class="next2">
                    </div>
                </div>
            </div>
        </div>
        <div class="steps">
            <div class="video__block">
                <video src="@/assets/img/steps.mp4" autoplay="" muted="" loop="" playsinline=""
                    poster="@/assets/img/steps.png">
                    <source src="@/assets/img/steps.mp4" type="video/mp4">
                    <img src="@/assets/img/steps.png" title="Your browser does not support the <video> tag">

                </video>
                <img src="@/assets/img/steps2.png" alt="" class="stepsimg howpc">
                <img src="@/assets/img/steps3.svg" alt="" class="stepsimg howmob">
            </div>
        </div>
        <a name="faq"></a>
        <div class="faq">
            <h1>частые вопросы</h1>

            <div class="faq__body">
                <div class="faq__left">
                    <button v-for="faq in faqs" :key="faq.id" :data-target="'#collapseExample' + faq.id"
                        aria-expanded="false" :aria-controls="'collapseExample' + faq.id" @click="toggleCollapse(faq.id)">
                        <div class="inside">
                            <span :style="{ 'font-weight': faq.isOpen ? '500' : '400' }">
                                {{ faq.question }}
                            </span>
                            <img src="@/assets/img/plus.svg" :class="{ rotate: faq.isOpen }"
                                style="transition: all .3s ease;" alt="">
                        </div>

                        <div :class="{ collapse: !faq.isOpen }" style="transition:  all .3s ease;"
                            :id="'collapseExample' + faq.id">
                            <span class="mt-3 trep">Нет, к сожалению Steam не предоставляет никаких способов для вывода
                                средств с
                                их счета.</span>
                        </div>
                    </button>
                </div>

                <div class="faq__right">
                    <video autoplay="" muted="" loop="" playsinline="" poster="@/assets/img/faq1.png">
                        <source src="@/assets/img/faq1.webm" type="video/webm">
                        <img src="@/assets/img/faq1.png" title="Your browser does not support the <video> tag">
                    </video>
                    <video autoplay="" muted="" loop="" playsinline="" poster="@/assets/img/faq2.png">
                        <source src="@/assets/img/faq2.mp4" type="video/mp4">
                        <img src="@/assets/img/faq2.png" title="Your browser does not support the <video> tag">
                    </video>
                    <video autoplay="" muted="" loop="" playsinline="" poster="@/assets/img/faq3.png">
                        <source src="@/assets/img/faq3.webm" type="video/webm">
                        <img src="@/assets/img/faq3.png" title="Your browser does not support the <video> tag">

                    </video>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { Navigation } from 'swiper/modules';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/navigation';
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    components: {
        Swiper,
        SwiperSlide,
    },
    data() {
        return {
            faqs: [
                { id: 1, isOpen: false, question: 'Что делать, если после пополнения баланса деньги не поступили на счет?' },
                { id: 2, isOpen: false, question: 'Что делать, если предмет не пришел или вы не можете его забрать?' },
                { id: 3, isOpen: false, question: 'Что делать, если к вам в стим добавился “наш сотрудник” и предлагает вам двойной баланс за ваши вещи?' },
                { id: 4, isOpen: false, question: 'Можно ли перевести средства со счета Steam?' },
                { id: 5, isOpen: false, question: 'Как продать или вывести предмет с сайта?' },
            ],
            breakpoints: {
                320: {
                    slidesPerView: 2,
                    spaceBetween: 20,

                },
                1025: {
                    slidesPerView: 3,
                    spaceBetween: 30,
                },
                1300: {
                    slidesPerView: 4,
                    spaceBetween: 30,
                },
                1600: {
                    slidesPerView: 6,
                    spaceBetween: 30,
                }
            },
            modules: [Navigation],
            navigation: {
                nextEl: '.next',
                prevEl: '.prev'
            },
            navigation2: {
                nextEl: '.next2',
                prevEl: '.prev2'
            },
            catalog: [],
            pathUrl: 'https://rustshop.kz',
        }
    },
    computed: {
        randomItems() {
            const items = [...this.catalog.results];

            for (let i = items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [items[i], items[j]] = [items[j], items[i]];
            }


            return items.slice(0, 12);
        },
        randomItemsPop() {

            const items = [...this.catalog.results];


            for (let i = items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [items[i], items[j]] = [items[j], items[i]];
            }


            return items.slice(12, 24);
        }
    },
    methods: {

        toggleCollapse(id) {
            const faq = this.faqs.find(f => f.id === id);
            faq.isOpen = !faq.isOpen;
        },
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
    }
    ,
    mounted() {
        this.getCatalog()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Главная | RustShop',
    ogTitle: 'Главная | RustShop',
    description: 'Главная | RustShop',
    ogDescription: 'Главная | RustShop',
})
</script>
<style lang="scss" scoped>
.page {

    .faq {
        margin-top: 85px;
        padding: 0 70px 116px 200px;

        @media (max-width: 1600px) {
            padding: 0 50px 50px 200px;
        }

        @media (max-width: 1024px) {
            margin-top: 20px;
            padding: 0 20px 50px;
        }

        .faq__body {
            display: flex;
            align-items: flex-start;
            gap: 40px;

            @media (max-width: 1660px) {
                flex-direction: column;
            }

            @media (max-width: 1024px) {
                gap: 20px;
            }

            .faq__right {
                display: flex;
                gap: 19px;

                @media (max-width: 1024px) {
                    gap: 9px;
                }

                video {
                    border-radius: 20px;
                    width: 14.271vw;
                    height: 100%;

                    @media (max-width: 1660px) {
                        width: auto;
                        max-width: 32.3%;

                    }

                    @media (max-width: 1024px) {
                        height: 203px;
                        object-fit: cover;
                        max-width: 31.3%;
                    }
                }
            }

            .faq__left {
                width: 100%;
                display: flex;
                flex-direction: column;
                gap: 30px;

                @media (max-width: 1024px) {
                    gap: 20px;
                }

                .rotate {
                    transform: rotate(30deg) !important;
                }

                button {
                    width: 100%;
                    text-align: left;
                    padding: 20px 10px;
                    border-radius: 10px;
                    background: #212322;
                    box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                    border: 0;
                    z-index: 2;
                    position: relative;
                    outline: none;
                    box-shadow: none;

                    @media (max-width: 1024px) {
                        padding: 15px 10px;
                    }

                    .inside {
                        display: flex;
                        justify-content: space-between;
                        align-items: center;
                        gap: 20px;
                        height: auto;

                    }

                    .trep {
                        @media (max-width: 1024px) {
                            font-size: 12px !important;
                        }
                    }

                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                        display: block;
                    }

                    .collapse {
                        display: none;
                        transition: height 0.3s ease-in-out;
                    }

                    .collapse.show {
                        display: block;
                    }
                }
            }
        }

        h1 {
            font-size: 32px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 41.6px */
            letter-spacing: 1.6px;
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
            margin: 0 0 30px;

            @media (max-width: 1024px) {
                font-size: 20px;
                margin: 0 0 20px;
            }
        }
    }

    .steps {
        margin-top: 66px;
        padding: 0 70px 0 200px;

        @media (max-width: 1600px) {
            padding: 0 50px 0 200px;
        }

        @media (max-width: 1024px) {
            margin-top: 20px;
            padding: 0;
        }

        .howpc {
            display: block;

            @media (max-width: 1024px) {
                display: none;
            }
        }

        .howmob {
            display: none;

            @media (max-width: 1024px) {
                display: block;
            }
        }

        .video__block {
            border-radius: 20px;
            position: relative;

            video {
                border-radius: 20px;
                width: 100%;
                z-index: 1;

                @media (max-width: 1024px) {
                    height: 405px;
                    object-fit: cover;
                }
            }

            .stepsimg {
                position: absolute;
                z-index: 2;
                left: 8%;
                top: 15%;
                max-width: 90%;

                @media (max-width: 1024px) {
                    max-width: 100%;
                    top: 5%;
                    left: 40%;

                }

                @media (max-width: 600px) {
                    left: 27%;
                }

                @media (max-width: 500px) {
                    left: 15%;
                }
            }
        }
    }

    .populars {
        margin-top: -50px;

        @media (max-width: 1024px) {
            margin-top: 0;
        }
    }

    .about {
        z-index: 40;
        height: 100vh;
        background-image: url('@/assets/img/salesbg.png');
        mix-blend-mode: lighten;
        background-color: transparent;
        position: relative;
        margin-top: -250px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        @media (max-width: 1024px) {
            background-image: none;
            margin: 0;
            height: auto;
            display: block;
            mix-blend-mode: screen;
        }


        .about__body {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            height: 100%;
            padding: 0 70px 0 200px;

            .blin {
                display: none;

                @media (max-width: 1024px) {
                    display: block;
                    width: 100%;
                    height: 220px;
                    border-radius: 20px;
                }
            }

            @media (max-width: 1600px) {
                padding: 0 50px 0 200px;
            }

            @media (max-width: 1024px) {
                padding: 0 20px;

                flex-direction: column;
                justify-content: flex-start;
                align-items: flex-start;
            }

            div:last-child {
                margin-top: 550px;

                @media (max-width: 1024px) {
                    margin-top: 20px;
                }
            }

            div {
                @media (max-width: 1024px) {
                    width: 100%;
                }
            }

            h1 {
                font-size: 32px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 41.6px */
                letter-spacing: 1.6px;
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;
                margin: 0 0 30px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                    margin: 0 0 20px;
                }
            }

            p {
                font-size: 15px;
                font-style: normal;
                font-weight: 300;
                line-height: 140%;
                font-family: var(--unb);
                color: #fff;
                margin: 0;
                max-width: 427px;

                @media (max-width: 1024px) {
                    font-size: 16px;
                    max-width: 100%;
                }
            }
        }
    }

    .sales {
        position: relative;
        z-index: 50;

        .sales__body {
            padding: 20px 70px 0 200px;
            position: relative;
            z-index: 2;

            @media (max-width: 1600px) {
                padding: 20px 50px 0 200px;
            }

            @media (max-width: 1024px) {
                padding: 20px 20px 30px;
            }

            .sales__slider {
                .navs {
                    display: flex;
                    justify-content: flex-end;
                    gap: 20px;
                    margin-top: 30px;

                    img {
                        cursor: pointer;
                    }
                }

                .slide {
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
        }



        h1 {
            font-size: 32px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 41.6px */
            letter-spacing: 1.6px;
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
            margin: 0 0 10px;

            @media (max-width: 1024px) {
                font-size: 20px;
            }
        }

        p {
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #C8C8C8;
            margin: 0 0 20px;
        }
    }

    .main {
        overflow: hidden;
        padding: 318px 70px 135px 200px;
        // height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background-image: url('@/assets/img/mainbg.png');
        mix-blend-mode: lighten;
        background-repeat: no-repeat;
        z-index: 5;
        position: relative;

        @media (max-width: 1600px) {
            padding: 318px 50px 135px 200px;
        }

        @media (max-width: 1024px) {
            padding: 83px 20px 92px;
            margin-top: 120px;
            border-radius: 20px;
            height: 720px;
            background-size: cover;
            justify-content: space-between;
        }

        .xyz {
            position: absolute;
            width: 100%;
            bottom: -50%;
            z-index: 1;
            background: #000;
            opacity: 0.6;
            filter: blur(129px);
        }

        .links {
            display: flex;
            justify-content: center;
            flex-direction: column;
            margin-top: 136px;
            z-index: 5;
            position: relative;

            @media (max-width: 1024px) {
                margin-top: 0;
            }

            p {
                font-size: 20px;
                font-style: normal;
                font-weight: 300;
                line-height: 130%;
                font-family: var(--unb);
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }

            .buttons {
                margin-top: 30px;
                display: flex;
                justify-content: center;
                gap: 30px;

                @media (max-width: 1024px) {
                    gap: 20px;
                }

                a {
                    text-decoration: none;
                    border-radius: 7px;
                    background: #75382E;
                    box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                    padding: 10px 3.125vw;

                    font-size: 15px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    text-transform: uppercase;
                    font-family: var(--unb);
                    color: #fff;

                    @media (max-width: 1024px) {
                        flex: 1;
                    }

                    &:last-child {
                        border-radius: 7px;
                        border: 1px solid #75382E;
                        background: #212322;
                        box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                    }
                }
            }
        }
    }
}
</style>