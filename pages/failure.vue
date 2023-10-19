<template>
    <div class="page">
        <div class="text-center">
            <h1>Неудачная транзакция</h1>
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
        }
    }
    ,
    methods: {
        sendRequest(reference) {
            const token = this.getAuthorizationCookie();
            const path = `${this.pathUrl}/api/money/failure/${reference}`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    if (response.status == 200) {
                        //  window.location.href = '/'
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
    mounted() {
        const url = window.location.href;
        const match = url.match(/order_pay_rustshop_(\d+)/);

        if (match) {
            this.extractedValue = match[0];
            console.log(this.extractedValue);

            this.sendRequest(match[0])
        }
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Неудачная транзакция | RustShop',
    ogTitle: 'Неудачная транзакция | RustShop',
    description: 'Неудачная транзакция | RustShop',
    ogDescription: 'Неудачная транзакция | RustShop',
})
</script>
<style lang="scss" scoped>
.page {
    height: 100vh;
    justify-content: center;
    align-items: center;
    display: flex;

    h1 {
        font-size: 32px;
        font-family: var(--unb);
        color: #fff;
        text-transform: uppercase;
        font-weight: 500;

        @media (max-width: 1024px) {
            font-size: 16px;
        }
    }
}
</style>