<template>
    <div class="page">
        <h1>Каталог</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" class="form-control" v-model="search" @input="searchProducts"
                    placeholder="Поиск по товарам" aria-label="Поиск по товарам" aria-describedby="basic-addon1">
            </div>

            <div class="buttons">
                <button @click.stop="toggleFilter()">Фильтр</button>
                <button @click.stop="toggleSort()">Сортировка</button>
            </div>
            <div class="sort__body" :class="{ activeFil: sort }">
                <h2 @click="sortActive = 1, sortBy('price')" :class="{ activeh: sortActive == 1 }">Сначала дешевле</h2>
                <h2 @click="sortActive = 2, sortBy('-price')" :class="{ activeh: sortActive == 2 }">Сначала дороже</h2>
                <h2 @click="sortActive = 3, sortBy('rating')" :class="{ activeh: sortActive == 3 }">Полулярное</h2>
                <h2 @click="sortActive = 4, sortBy('-rating')" :class="{ activeh: sortActive == 4 }">Новое</h2>
            </div>
            <div class="filter__body" @click="stopPropagation" :class="{ activeFil: filter }">
                <div class="w-100">
                    <h2>Категория:</h2>
                    <div class="rare">
                        <div class="rare__gap">
                            <label class="custom-checkbox" v-for="item in filters.Category" :key="item">
                                <input type="checkbox" v-model="selectedCategory" :value="item" @change="applyFilters">
                                <p class="checkbox-text m-0">{{ item }}
                                </p>
                            </label>
                        </div>
                    </div>
                </div>
                <div class="types">
                    <div class="select">
                        <h2>тип предмета:</h2>
                        <select name="" id="" v-model="selectedType" @change="applyFilters">
                            <option value="" disabled selected>Все</option>
                            <option v-for="item in filters.ItemType" :key="item">{{ item }}</option>
                        </select>
                    </div>
                    <div>
                        <h2>Цена</h2>
                        <div class="price">
                            <input type="number" id="from" name="from" placeholder="от" v-model="minPrice"
                                @input="applyFilters">
                            <img src="@/assets/img/line.svg" alt="">
                            <input type="number" id="to" name="to" placeholder="до" v-model="maxPrice"
                                @input="applyFilters">
                        </div>
                    </div>
                </div>
            </div>
        </div>


        <div class="catalog__items">
            <NuxtLink v-for="item in catalog.results" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
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
        <div class="text-right">
            <button ref="showmore" @click="loadMoreProducts()">показать еще</button>
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
            filter: false,
            sort: false,
            sortActive: 0,
            catalog: [],
            pathUrl: 'https://rustshop.kz',
            kzt: null,
            usd: null,
            search: '',
            filters: [],
            selectedRarity: [],
            selectedHero: '',
            selectedCell: '',
            minPrice: null,
            maxPrice: null,
            selectedCategory: [],
            selectedType: '',
        }
    },
    methods: {
        loadMoreProducts() {
            if (this.catalog.links.next) {
                this.$refs.showmore.innerHTML = 'Загружаем'
                axios
                    .get(this.catalog.links.next)
                    .then(response => {
                        this.$refs.showmore.innerHTML = 'Показать еще'
                        this.catalog.results.push(...response.data.results);
                        this.catalog.links.next = response.data.links.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        getCatalog() {
            const url = `${this.pathUrl}/api/products/catalog`

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch(error => {
                    console.error(error);
                });
        },
        searchProducts() {
            const query = this.search.trim();
            if (query) {
                const queryParams = `?search=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getCatalog();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/catalog${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch(error => {
                    console.error(error);
                });
        },
        applyFilters() {
            let queryParams = '';
            if (this.selectedCategory) {
                queryParams += `&tags__category=${this.selectedCategory}`;
            }
            if (this.selectedType) {
                queryParams += `&tags__itemtype=${this.selectedType}`;
            }
            if (this.minPrice !== null) {
                queryParams += `&min_price=${this.minPrice}`;
            }
            if (this.maxPrice !== null) {
                queryParams += `&max_price=${this.maxPrice}`;
            }

            // Remove the leading '&' if there are any query parameters
            if (queryParams) {
                queryParams = `?${queryParams.substring(1)}`;
            }

            const path = `${this.pathUrl}/api/products/catalog${queryParams}`;
            axios
                .get(path)
                .then((response) => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch((error) => {
                    console.error(error);
                });
        },
        sortBy(ordering) {
            this.sort = false
            const path = `${this.pathUrl}/api/products/catalog?ordering=${ordering}`;
            axios
                .get(path)
                .then(response => {
                    this.catalog = response.data;

                })
                .catch(error => {
                    console.error(error);
                });
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
        this.getCatalog()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Каталог | RustShop',
    ogTitle: 'Каталог | RustShop',
    description: 'Каталог | RustShop',
    ogDescription: 'Каталог | RustShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 73px 70px 60px 200px;

    @media (max-width: 1600px) {
        padding: 73px 50px 50px 200px;
    }

    @media (max-width: 1024px) {
        padding: 140px 20px 50px;
    }

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
                margin: 0;
                border-radius: 7px;
                background: #75382E;
                box-shadow: 0px 4px 30px 0px rgba(0, 0, 0, 0.25);
                padding: 13px 30px;
                border: 0;

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