<template>
    <div class="page">
        <h1>Личный кабинет</h1>

        <div class="navs">
            <span :class="{ activeSpan: activeTab == 0 }" @click="activeTab = 0">Личные данные</span>
            <span :class="{ activeSpan: activeTab == 1 }" @click="activeTab = 1">Транзакции</span>
            <span :class="{ activeSpan: activeTab == 2 }" @click="activeTab = 2">Баланс аккаунта</span>
        </div>

        <div class="account" v-if="activeTab == 0">
            <div class="account__left">
                <div class="link">
                    <h2>{{ userData.steam_personaname || userData.first_name }}</h2>
                    <a :href="userData.steam_profileurl" v-if="userData.steam_profileurl" target="_blank">
                        <img src="@/assets/img/steam.svg" alt="">
                        <span>{{ userData.steam_personaname }}</span>
                    </a>
                </div>

                <div class="password">
                    <h2>Пароль</h2>

                    <div>
                        <label for="password">Новый пароль</label>
                        <input type="password" name="password" id="password" v-model="password">
                    </div>
                    <div>
                        <label for="password">Повторите пароль</label>
                        <input type="password" name="repeat_password" id="repeat_password" v-model="repeat_password">
                    </div>
                    <button @click="editUser()">Сохранить</button>
                    <button @click="logOut()" class="d-block mt-4">Выйти</button>
                </div>
            </div>

            <div class="account__right">
                <div>
                    <label for="trade_link" class="trade_link">
                        <div>
                            <img src="@/assets/img/link.svg" alt="">
                            <span>Трейд-ссылка</span>
                        </div>
                        <a href="https://steamcommunity.com/">Где получить?</a>
                    </label>
                    <input type="url" name="trade_link" id="trade_link" v-model="trade_link">
                </div>
                <button @click="editUser()">сохранить</button>
                <div class="email">
                    <label for="email">E-mail</label>
                    <input type="email" name="email" id="email" v-model="email">
                </div>
                <button @click="editUser()">сохранить</button>
            </div>
        </div>

        <div class="transactions" v-if="activeTab == 1">
            <div class="empty" v-if="transactions.length <= 0">
                <h1 class="text-center">Список операций будет доступен, когда Вы пополните или выведете средства с баланса
                </h1>

                <div class="buttons">
                    <NuxtLink to="/catalog">Магазин</NuxtLink>
                    <NuxtLink to="/sale">Продажа</NuxtLink>
                </div>
            </div>
            <div class="scrollik" v-else>

                <div class="mobtrans">
                    <div class="mobbody">
                        <div class="mob__item" v-for="item in transactions" :key="item.id">
                            <div class="imageblock">
                                <div class="counts">
                                    <small>Тип</small>
                                    <span>{{ item.type }}</span>
                                </div>
                                <small>Предмет</small>
                                <div class="item text-left">
                                    <img :src="item.item.img" alt="">
                                    <span>{{ item.item.name }}</span>
                                </div>
                            </div>
                            <div class="countsblock">

                                <div class="counts">
                                    <small>Цена</small>
                                    <span>{{ item.item.sell_price.toFixed(1).toLocaleString() }}₸</span>
                                </div>

                                <div class="counts">
                                    <small>Дата</small>
                                    <span>{{ formatDate(item.date) }}</span>
                                </div>
                                <div class="counts">
                                    <small>Статус</small>
                                    <span
                                        :class="{ success: item.status == 'Совершено', process: item.status == 'В процессе', failure: item.status == 'Отменено' }">{{
                                            item.status }}</span>
                                </div>
                            </div>
                        </div>



                    </div>
                </div>
                <table class="table table-borderless text-center">
                    <thead>
                        <tr>
                            <th scope="col">Тип</th>
                            <th scope="col">Предмет</th>
                            <th scope="col">Цена</th>
                            <th scope="col">Дата</th>
                            <th scope="col">Статус</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in transactions" :key="item.id">
                            <td>{{ item.type }}</td>
                            <td class="d-flex justify-content-center">
                                <div class="transitem">
                                    <img :src="item.item.img" alt="">
                                    <span>{{ item.item.name }}</span>

                                </div>
                            </td>
                            <td>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</td>
                            <td>{{ formatDate(item.date) }}</td>
                            <td
                                :class="{ success: item.status == 'Совершено', process: item.status == 'В процессе', failure: item.status == 'Отменено' }">
                                {{ item.status }}</td>
                        </tr>

                    </tbody>
                </table>
            </div>
        </div>

        <div class="balance" v-if="activeTab == 2">
            <div class="empty" v-if="balance.length <= 0">
                <h1 class="text-center">Список операций будет доступен, когда Вы пополните или выведете средства с баланса
                </h1>
            </div>
            <div class="scrollik" v-else>

                <div class="mobtrans">

                    <div class="mob__body" v-for="item in balance.slice().reverse()" :key="item.id">
                        <div class="paddingblya">
                            <div class="body__header">
                                <small>{{ item.type_operation }}</small>
                                <span>{{ formatDate(item.date) }}</span>
                                <img src="@/assets/img/success.svg" v-if="item.paid == 1" alt="">
                                <img src="@/assets/img/failure.svg" v-else alt="">
                            </div>
                            <div class="body__footer">
                                <div>
                                    <small>Цена</small>
                                    <span> {{ item.amount }} ₸</span>
                                </div>
                                <div>
                                    <small>Состояние счета</small>
                                    <span>{{ item.amount_now.toFixed(1) }} ₸</span>
                                </div>
                            </div>
                        </div>
                        <hr>
                    </div>
                </div>
                <table class="table table-borderless text-center">
                    <thead>
                        <tr>
                            <th scope="col">Тип</th>
                            <th scope="col">Цена</th>
                            <th scope="col">Дата и время</th>
                            <th scope="col">Статус</th>
                            <th scope="col">Состояние счета</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in balance.slice().reverse()" :key="item.id">
                            <td>{{ item.type_operation }}</td>
                            <td>{{ item.amount }} ₸</td>
                            <td>{{ formatDate(item.date) }}</td>
                            <td :class="{ success: item.paid == 1, failure: item.paid == 0 }">{{ item.paid == 1 ?
                                'Совершено'
                                : 'Отменено' }}</td>
                            <td>{{ item.amount_now.toFixed(1) }} ₸</td>
                        </tr>

                    </tbody>
                </table>
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
            activeTab: 2,
            password: '',
            repeat_password: '',
            trade_link: '',
            email: '',
            userData: [],
            transactions: [],
            balance: [],

        }
    },
    methods: {
        formatDate(dateTime) {
            const date = new Date(dateTime);

            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = String(date.getFullYear()).slice(-2);
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');

            return `${day}.${month}.${year} ${hours}:${minutes}`;
        },
        editUser() {
            const url = `${this.pathUrl}/api/users/edit-profile/${this.userData.id}`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            if (this.password == '') {
                axios
                    .put(url, {
                        email: this.email,
                        trade_link: this.trade_link,

                    })

                    .then((res) => {
                        console.log(res)

                        this.getUser()
                    })
            }
            else {
                axios
                    .put(url, {
                        email: this.email,
                        password: this.password,
                        trade_link: this.trade_link,

                    })

                    .then((res) => {
                        console.log(res)

                        this.getUser()
                    })
            }
        },
        getUser() {
            const url = `${this.pathUrl}/api/users/profile/`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.userData = res.data
                    this.trade_link = res.data.trade_link
                    this.email = res.data.email
                    this.transactions = res.data.transactions_item
                    this.balance = res.data.transactions_balance
                })
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getUser()

        }
        else {
            this.$router.push('/register')
        }

    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Личный кабинет | RustShop',
    ogTitle: 'Личный кабинет | RustShop',
    description: 'Личный кабинет | RustShop',
    ogDescription: 'Личный кабинет | RustShop',
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



    .balance {
        margin-top: 37px;
        // padding: 30px;
        border-radius: 10px;

        hr {
            margin: 20px 0;
            border-top: 1px solid #fff;
        }

        .empty {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 500px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--unb);
                text-transform: none;
                color: #fff;
            }
        }

        .scrollik {
            height: 700px;
            overflow-y: auto;

        }

        .scrollik {
            scrollbar-color: #999 #333;
        }

        .scrollik::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .scrollik::-webkit-scrollbar-thumb {
            background: #FF1D00;
            border-radius: 10px;
        }

        .scrollik::-webkit-scrollbar-track {
            background: #000;
            border-radius: 10px;
        }

        .mobtrans {
            display: none;

            // border-radius: 30px;
            // background: transparent;
            // box-shadow: 0px 0px 15px 0px rgba(47, 59, 163, 0.20);

            .mob__body {

                &:last-child {
                    hr {
                        display: none;
                    }
                }

                .paddingblya {

                    // padding: 30px;


                    .body__header {
                        display: flex;
                        justify-content: space-between;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: normal;
                            font-family: var(--mon);
                            color: #fff;
                        }

                        span {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--mon);
                            color: #fff;
                            white-space: nowrap;
                        }
                    }

                    .body__footer {
                        display: flex;
                        justify-content: space-between;
                        margin-top: 15px;
                        text-align: left;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: normal;
                            font-family: var(--mon);
                            color: #fff;
                            text-align: left;
                        }

                        span {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--mon);
                            color: #fff;
                            display: block;
                            text-align: left;
                            margin-top: 10px;
                            //   white-space: nowrap;
                        }

                    }
                }
            }

            @media (max-width: 1024px) {
                display: block;
            }
        }

        table {
            @media (max-width: 1024px) {
                display: none;
            }

            th {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--mon);
                color: #C8C8C8;
                opacity: 0.5;
            }

            .success {
                color: #05EA2A !important;
            }

            .failure {
                color: #EA0505 !important;
            }

            .process {
                color: #EAE105 !important;
            }

            .tradelock {
                color: #EF8641 !important;
            }

            td {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--mon);
                color: #fff;

                @media (max-width: 1600px) {
                    font-size: 16px;
                }

                .item {
                    max-width: 130px;
                    padding: 5px;
                    display: flex;
                    flex-direction: column;
                    border-radius: 10px;
                    border: 1.038px solid var(--Rare, #3348E0);
                    background: linear-gradient(90deg, #24212B 0%, rgba(36, 33, 43, 0.65) 100%);

                    span {
                        font-size: 8.306px;
                        font-style: normal;
                        font-weight: 600;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                        margin: 5px 0 5px;

                        &:last-child {
                            margin: 0;
                        }
                    }

                    img {
                        width: 119px;
                        height: 79px;
                    }
                }
            }
        }
    }

    .transactions {
        padding: 20px 30px;
        border-radius: 10px;
        margin-top: 37px;

        @media (max-width: 1400px) {
            padding: 10px;
        }

        .empty {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 500px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--unb);
                text-transform: none;
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 18px;
                }
            }

            .buttons {
                display: flex;
                gap: 30px;
                margin-top: 50px;

                a {
                    border-radius: 7px;
                    background: #75382E;
                    box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                    padding: 10px 3.125vw;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    transition: all .3s ease;
                    color: #fff;
                    text-decoration: none;

                    @media (max-width: 1024px) {
                        font-size: 16px;
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

        .scrollik {
            margin-top: 45px;
            height: 700px;
            overflow-y: auto;

            @media (max-width: 1400px) {
                overflow-y: unset;
                height: auto;
            }
        }

        .scrollik {
            scrollbar-color: #999 #333;
        }

        .scrollik::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .scrollik::-webkit-scrollbar-thumb {
            background: #0500FF;
            border-radius: 10px;
        }

        .scrollik::-webkit-scrollbar-track {
            background: #2A1310;
            border-radius: 10px;
        }

        .mobtrans {
            display: none;

            .mobbody {
                display: flex;
                flex-direction: column;
                gap: 20px;

                .mob__item {
                    display: flex;
                    gap: 20px;
                    flex-direction: column;

                    .countsblock {
                        display: flex;
                        flex-direction: column;
                        gap: 20px;
                    }

                    .imageblock {
                        img {
                            width: 110px;
                            height: 110px;
                        }

                        .counts {
                            margin-bottom: 17px;
                        }

                        .item {
                            margin-top: 15px;
                            display: flex;
                            align-items: center;
                        }
                    }

                    small,
                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #C8C8C8;
                        opacity: 0.5;
                        display: block;
                    }

                    span {
                        color: #fff !important;
                        opacity: 1 !important;
                    }

                    .success {
                        color: #05EA2A !important;
                    }

                    .failure {
                        color: #EA0505 !important;
                    }

                    .process {
                        color: #EAE105 !important;
                    }

                    .tradelock {
                        color: #EF8641 !important;
                    }


                    .counts {
                        display: flex;
                        align-items: center;
                        justify-content: space-between;
                        width: 100%;
                    }
                }
            }

            @media (max-width: 1400px) {
                display: block;
            }
        }

        table {
            @media (max-width: 1400px) {
                display: none;
            }

            th {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--mon);
                color: #C8C8C8;
                opacity: 0.5;
            }

            .success {
                color: #05EA2A !important;
            }

            .failure {
                color: #EA0505 !important;
            }

            .process {
                color: #EAE105 !important;
            }

            .tradelock {
                color: #EF8641 !important;
            }

            td {
                font-size: 16px;
                font-style: normal;
                font-weight: 300;
                line-height: normal;
                font-family: var(--mon);
                color: #fff;

                @media (max-width: 1600px) {
                    font-size: 16px;
                }

                .transitem {
                    display: flex;
                    gap: 10px;
                    align-items: center;

                    img {
                        width: 110px;
                        height: 110px;
                    }

                    span {
                        max-width: 258px;
                    }
                }
            }
        }

        .filters {
            display: flex;
            justify-content: space-between;

            @media (max-width: 1400px) {
                flex-direction: column;
                gap: 15px;
            }

            .shit {
                display: flex;
                gap: 64px;

                @media (max-width: 1400px) {
                    flex-direction: column;
                    gap: 15px;
                }

                .buttons {
                    display: flex;
                    gap: 30px;

                    button {
                        padding: 10px 20px;
                        border-radius: 10px;
                        border: 0;
                        background: var(--iris-100, #FF1D00);

                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                        transition: all .3s ease;

                        @media (max-width: 1400px) {
                            padding: 10px 0;
                            flex: 1;
                        }

                        &:hover {
                            border: 0;
                            background: #0500FF;
                        }

                        &:last-child {
                            border-radius: 10px;
                            border: 0;
                            background: #0500FF;

                            &:hover {
                                border: 0;
                                background: var(--iris-100, #FF1D00);
                            }
                        }
                    }
                }

                .selects {
                    display: flex;
                    gap: 40px;

                    @media (max-width: 1400px) {
                        justify-content: space-between;
                    }
                }

                select {
                    background: transparent;
                    color: #fff;
                    border: 0;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 32px;
                    font-family: var(--mon);
                    color: #fff;

                    @media (max-width: 1024px) {
                        max-width: 100%;
                        font-size: 12px;
                    }
                }
            }

            input {
                border-radius: 7px;
                border: 2px solid #452721;
                background: #212322;
                padding: 10px 20px 10px 0;
                border-left: 0;
                border-top-left-radius: 0;
                border-bottom-left-radius: 0;
                max-width: 590px;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 32px;
                font-family: var(--mon);
                color: #fff;
                box-shadow: none;

                @media (max-width: 1400px) {
                    max-width: 100%;
                }

                &::placeholder {
                    color: #fff;
                }
            }

            .input-group-text {
                border-radius: 7px;
                border: 2px solid #452721;

                background: #212322;
                border-right: 0;
                border-top-right-radius: 0;
                border-bottom-right-radius: 0;
            }
        }
    }

    .account {
        margin-top: 40px;

        display: flex;
        gap: 6.25vw;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 20px;
        }

        input {
            @media (max-width: 1024px) {
                width: 100% !important;
                max-width: 100% !important;
            }
        }

        button {
            padding: 10px 2.083vw;
            border-radius: 7px;
            background: #75382E;
            box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);

            font-size: 15px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 19.5px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
            border: 0;
            margin-top: 20px;

        }

        .account__right {

            margin-top: 90px;
            width: 795px;

            @media (max-width: 1024px) {
                width: 100% !important;
                margin-top: 20px;
            }

            .trade_link {
                margin-bottom: 12px;
                display: flex;
                justify-content: space-between;
                align-items: center;

                @media (max-width: 1024px) {
                    flex-direction: column;
                    gap: 13px;
                    align-items: flex-start;
                }

                div {
                    display: flex;
                    gap: 10px;
                    align-items: center;

                    span {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        text-transform: uppercase;
                        font-family: var(--unb);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 20px;

                        }
                    }
                }

                a {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    text-decoration-line: underline;
                    font-family: var(--mon);
                    color: #fff;
                    text-transform: none;

                    @media (max-width: 1024px) {
                        font-size: 16px;

                    }
                }
            }

            label,
            input {
                display: block;
            }

            .email {
                margin-top: 30px;

                input {
                    max-width: 573px;
                }
            }

            label {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;
                margin-bottom: 9px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                }
            }

            input {
                width: 100%;
                border-radius: 7px;
                border: 1px solid #75382E;

                background: #212322;
                padding: 12px 15px;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--mon);
                color: #fff;
            }
        }

        .account__left {
            width: 31.25vw;

            @media (max-width: 1024px) {
                width: 100% !important;
            }

            .password {
                margin-top: 60px;

                @media (max-width: 1024px) {
                    width: 100% !important;
                    margin-top: 30px;
                }

                label,
                input {
                    display: block;
                }

                label {
                    margin: 0 0 9px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;

                }

                input {
                    width: 100%;
                    border-radius: 7px;
                    border: 1px solid #75382E;

                    background: #212322;
                    padding: 12px 15px;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;

                    font-family: var(--mon);
                    color: #fff;
                    margin: 0 0 23px;
                }

                h2 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--unb);
                    color: #fff;
                    margin: 0 0 18px;

                    @media (max-width: 1024px) {
                        font-size: 20px;
                    }
                }


            }

            .link {
                display: flex;
                gap: 30px;
                align-items: center;

                h2 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--unb);
                    color: #fff;
                    margin: 0;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                a {
                    text-decoration: none;
                    display: flex;
                    gap: 10px;
                    align-items: center;

                    span {
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }
                }
            }
        }
    }

    .navs {
        display: flex;
        gap: 50px;

        @media (max-width: 1024px) {
            flex-wrap: wrap;
            gap: 20px;
        }

        .activeSpan {
            color: #fff;
            font-weight: 600 !important;
        }

        span {
            cursor: pointer;
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            text-transform: uppercase;
            font-family: var(--mon);
            color: #9B9B9B;

            @media (max-width: 1024px) {
                font-size: 16px;
                text-transform: none;
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
        margin: 0 0 40px;

        @media (max-width: 1024px) {
            font-size: 20px;
            margin: 0 0 20px;
        }
    }
}
</style>