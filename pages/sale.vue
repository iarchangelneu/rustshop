<template>
    <div class="page">
        <h1>Инвентарь</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" :disabled="!sales" class="form-control" placeholder="Поиск по товарам"
                    aria-describedby="basic-addon1" v-model="search" @input="searchProducts">
            </div>

            <div class="buttons">
                <button @click="getTrans(), sales = false" v-if="sales">Покупки</button>
                <button @click="getInventory(), sales = true" v-if="!sales">Инвентарь</button>
                <button @click="getSteam()" :disabled="!sales" ref="update">Обновить инвентарь</button>
            </div>
        </div>


        <div class="catalog__items">
            <div class="catalog__item" v-for="item in inventory.results" :key="item.id" v-if="sales">
                <!-- <span class="disc">-10%</span> -->
                <img :src="item.img" class="main__img" alt="">

                <h2>{{ item.name }}</h2>

                <div class="price">
                    <div class="out">
                        <div class="sitee" @click="openModal(item.id)">
                            <img src="@/assets/img/onsite.svg" alt="">
                            <img src="@/assets/img/banner1.svg" alt="" class="banner1">
                        </div>
                    </div>
                    <span>{{ item.price.toFixed(1).toLocaleString() }} ₸</span>
                </div>
            </div>
            <div class="catalog__item" v-for="item in inventory" :key="item.id" v-if="!sales">
                <!-- <span class="disc">-10%</span> -->
                <img :src="item.img" class="main__img" alt="">

                <h2>{{ item.name }}</h2>

                <div class="price">
                    <div class="out">
                        <div class="sitee" @click="openModal2(item.id)">
                            <img src="@/assets/img/onsite.svg" alt="">
                            <img src="@/assets/img/banner1.svg" alt="" class="banner1">
                        </div>
                        <div class="steamee" @click="openSteam2(item.id)">
                            <img src="@/assets/img/insteam.svg" alt="">
                            <img src="@/assets/img/banner2.svg" alt="" class="banner2">
                        </div>
                    </div>
                    <span>{{ item.price.toFixed(1).toLocaleString() }} ₸</span>
                </div>
            </div>
        </div>
        <div class="text-right" v-if="sales">
            <button ref="showmore" @click="loadMoreProducts()">показать еще</button>
        </div>
    </div>
    <ModalFirst :product="modal"></ModalFirst>
    <SteamModal :product="modal"></SteamModal>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            filter: false,
            sort: false,
            pathUrl: 'https://rustshop.kz',
            inventory: [],
            kzt: null,
            usd: null,
            modal: {},
            sales: true,
            search: '',
        }
    },
    methods: {
        loadMoreProducts() {
            if (this.inventory.links.next) {
                this.$refs.showmore.innerHTML = 'Загружаем'
                axios
                    .get(this.inventory.links.next)
                    .then(response => {
                        this.$refs.showmore.innerHTML = 'Показать еще'
                        this.inventory.results.push(...response.data.results);
                        this.inventory.links.next = response.data.links.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        openModal(id) {
            this.modal = this.inventory.results.filter(item => item.id == id)[0];
            $('#modalFirst').modal('show')

        },
        openSteam(id) {
            this.modal = this.inventory.results.filter(item => item.id == id)[0];
            $('#steamModal').modal('show')
        },
        openModal2(id) {
            this.modal = this.inventory.filter(item => item.id == id)[0];
            $('#modalFirst').modal('show')

        },
        openSteam2(id) {
            this.modal = this.inventory.filter(item => item.id == id)[0];
            $('#steamModal').modal('show')
        },
        getTrans() {
            const url = `${this.pathUrl}/api/users/profile/`;
            const token = this.getAuthorizationCookie();
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res);
                    this.inventory = res.data.transactions_item.map(item => item.item);
                });
        },
        getSteam() {
            const url = `${this.pathUrl}/api/products/update-inventory`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            this.$refs.update.innerHTML = 'Загружаем'

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getInventory()
                    if (res.status == 200) {
                        this.$refs.update.innerHTML = 'Обновить инвентарь'
                    }
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        searchProducts() {
            const query = this.search.trim();
            if (query) {
                const queryParams = `?search=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getInventory();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/get-inventory${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.inventory = response.data
                })
                .catch(error => {
                    console.error(error);
                });
        },

        getInventory() {
            const url = `${this.pathUrl}/api/products/get-inventory`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.inventory = res.data
                })
        },
        toggleFilter() {
            this.filter = !this.filter;
            this.sort = false
            if (this.filter) {
                document.addEventListener('click', this.closeFilter);
            } else {
                document.removeEventListener('click', this.closeFilter);
            }
        },
        closeFilter() {
            this.filter = false;
            document.removeEventListener('click', this.closeFilter);
        },
        toggleSort() {
            this.sort = !this.sort;
            this.filter = false
            if (this.sort) {
                document.addEventListener('click', this.closeSort);
            } else {
                document.removeEventListener('click', this.closeSort);
            }
        },
        closeSort() {
            this.sort = false;
            document.removeEventListener('click', this.closeSort);
        },
        stopPropagation(event) {
            event.stopPropagation();
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getInventory()
        }
        else {
            this.$router.push('/register')
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Инвентарь | RustShop',
    ogTitle: 'Инвентарь | RustShop',
    description: 'Инвентарь | RustShop',
    ogDescription: 'Инвентарь | RustShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 73px 70px 60px 200px;


    button {
        margin-top: 20px;
        border-radius: 7px;
        background: #75382E;
        box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
        padding: 10px 1.927vw;
        border: 0;

        font-size: 15px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        /* 19.5px */
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
    }

    @media (max-width: 1600px) {
        padding: 73px 50px 50px 200px;
    }

    @media (max-width: 1024px) {
        padding: 140px 20px 50px;
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
                align-items: center;
                gap: 6px;
                justify-content: space-between;

                .out {
                    display: flex;
                    gap: 20px;

                    .sitee {
                        position: relative;
                        cursor: pointer;

                        .banner1 {
                            position: absolute;
                            top: -240%;
                            left: -180%;
                            transition: all .3s ease;
                            transform: scale(0);
                        }
                    }

                    .steamee {
                        position: relative;
                        cursor: pointer;

                        .banner2 {
                            position: absolute;
                            top: -240%;
                            left: -180%;
                            transition: all .3s ease;
                            transform: scale(0);
                        }
                    }

                    .sitee:hover .banner1 {
                        transform: scale(1) !important;
                    }

                    .steamee:hover .banner2 {
                        transform: scale(1) !important;
                    }
                }

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

    h1 {
        font-size: 32px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        /* 41.6px */
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
        margin: 0 0 30px;

        @media (max-width: 1024px) {
            font-size: 20px;
            margin: 0 0 20px;
        }
    }


    .filters {
        display: flex;
        gap: 30px;
        position: relative;
        align-items: center;

        @media (max-width: 1024px) {
            gap: 20px;
            flex-direction: column;
        }

        .activeFil {
            transform: scale(1) !important;
        }

        .sort__body {
            position: absolute;
            left: 47.5%;
            top: 0;
            transform: scale(0);
            transition: all .3s ease;
            margin-top: 70px;
            border-radius: 10px;
            border: 1px solid #75382E;
            background: #212322;
            padding: 20px;
            text-align: center;
            z-index: 5;


            @media (max-width: 1024px) {
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            .activeh {
                color: #75382E;
            }

            h2 {
                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--mon);
                color: #fff;
                margin: 0 0 20px;
                cursor: pointer;
                transition: all .3s ease;

                &:last-child {
                    margin: 0;
                }
            }
        }

        .filter__body {
            position: absolute;
            transform: scale(0);
            transition: all .3s ease;
            left: 19.6%;
            top: 0;
            margin-top: 70px;
            border-radius: 10px;
            border: 1px solid #75382E;
            width: 680px;

            background: #212322;
            padding: 20px;
            display: flex;
            z-index: 5;
            gap: 71px;

            @media (max-width: 1024px) {
                flex-direction: column;
                gap: 10px;
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            .price {
                display: flex;
                gap: 4px;
                align-items: center;

                input {
                    border-radius: 5px;
                    border: 1px solid #75382E;

                    background: #000;
                    padding: 3px 5px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                    max-width: 130px;
                    height: 30px;


                    &::placeholder {
                        color: #fff;
                    }
                }
            }

            .types {
                .select {
                    margin: 0 0 30px;
                }

                h2 {
                    margin: 0 0 10px !important;
                }

                select {
                    border-radius: 5px;
                    border: 1px solid #75382E;

                    background: #000;
                    color: #fff;
                    padding: 5px 10px;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
                    width: 100%;

                    @media (max-width: 1024px) {
                        width: 100%;
                    }
                }
            }

            h2 {

                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--mon);
                margin: 0 0 20px;
                color: #fff;

                &:last-child {
                    margin: 0 0 10px;
                }
            }

            .rare {
                width: 100%;


                .rare__gap {
                    display: grid;
                    gap: 20px;
                    grid-auto-flow: dense;
                    grid-template-columns: repeat(auto-fill, minmax(128px, 1fr));
                }

                label {
                    display: block;
                    margin: 0;
                }

                div {
                    p {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #fff;
                    }
                }
            }

        }

        .input-group {
            max-width: 620px;

            @media (max-width: 1024px) {
                max-width: 100%;
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

        input {
            border-radius: 7px;
            border: 2px solid #452721;

            background: #212322;
            border-left: 0;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
            padding: 10px 20px 10px 0;
            box-shadow: none;

            font-size: 20px;
            font-style: normal;
            font-weight: 300;
            line-height: 32px;
            font-family: var(--mon);
            color: #fff;

            &::placeholder {
                color: #fff;
            }
        }

        .buttons {
            display: flex;
            gap: 30px;
            align-items: center;

            @media (max-width: 1024px) {
                gap: 20px;
                width: 100%;
            }

            button {
                border-radius: 7px;
                background: #75382E;
                box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                padding: 13px 30px;
                border: 0;
                margin: 0;

                font-size: 15px;
                text-transform: uppercase;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--unb);
                color: #fff;

                @media (max-width: 1024px) {
                    flex: 1;
                    padding: 10px 0;
                    font-size: 16px;
                }
            }
        }
    }

    .custom-checkbox p,
    .custom-checkbox a {
        @media (max-width: 1024px) {
            font-size: 12px !important;
        }
    }

    .custom-checkbox input[type="checkbox"] {
        opacity: 0;
        position: absolute;
        border-radius: 3px;
    }

    .custom-checkbox .checkbox-text:before {
        content: '';
        display: inline-block;
        vertical-align: middle;
        width: 20px;
        height: 20px;
        margin-right: 10px;
        border: 2px solid #C8C8C8;
        border-radius: 3px;
        margin-bottom: 3px;
    }

    .custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
        content: url('@/assets/img/check.svg');
        font-size: 10px;
        color: black;
        text-align: center;
        line-height: 20px;
        background: transparent;
    }

    .custom-checkbox {
        margin-bottom: 20px;

    }

    .custom-checkbox:last-child {
        margin-bottom: 0 !important;

        @media (max-width: 1024px) {
            margin-bottom: 22px !important;
        }
    }
}
</style>