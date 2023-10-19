<template>
    <div class="modal fade" id="modalSecond" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <div class="text-center">
                        <h3>Успешно!</h3>

                        <h1><strong>{{ name }}</strong> выставлен на продажу</h1>
                        <p>Если предмет будет продан - Ваш баланс на сайте обновится </p>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        name: String,
    },
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
        font-weight: 300;
        line-height: 130%;
        font-family: var(--unb);
        color: #fff;
        margin: 20px 0 18px;
    }

    h3 {
        font-size: 32px;
        font-style: normal;
        font-weight: 500;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
    }

    p {
        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        margin: 0;
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