<template>
    <div class="modal fade" id="emailModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <div class="text-center">
                        <h1>вывести предмет в steam</h1>
                        <p style="color: red; font-weight: 500;">ОШИБКА</p>
                        <p>Чтобы вывести предмет в Steam, необходимо привязать E-mail в Личном кабинете</p>

                        <NuxtLink to="/account">Личный кабинет</NuxtLink>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {}
    }
}
</script>

<style lang="scss" scoped>
.modal-body {
    padding: 0;

    h1 {
        font-size: 20px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        /* 31.2px */
        letter-spacing: 1.2px;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
        margin: 0 0 30px;

        @media (max-width: 1024px) {
            text-align: center;
            font-size: 16px;
            margin: 0 0 20px;
        }
    }

    a {
        border-radius: 7px;
        border: 0;
        background: var(--iris-100, #75382E);
        padding: 10px 20px;
        display: inline-block;
        text-decoration: none;

        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
    }

    p {
        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        margin: 0 0 18px;
    }
}

.modal-content {
    border-radius: 10px;
    border: 1px solid #75382E;
    background: #212322;
    box-shadow: 0px 4px 38px 0px rgba(117, 56, 46, 0.30);
    padding: 20px 20px 50px;
}

.modal-dialog {
    max-width: 596px;
    margin: 1.75rem auto;
}
</style>