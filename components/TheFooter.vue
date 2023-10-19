<template>
    <footer v-if="!hideFooterOnPages.includes($route.name)">
        <hr>
        <div class="footer__body">
            <NuxtLink to="/">
                <img src="@/assets/img/footerlogo.svg" alt="">
            </NuxtLink>

            <div class="links">
                <div>
                    <NuxtLink to="/catalog">Магазин</NuxtLink>
                    <NuxtLink to="/sale">продажа</NuxtLink>
                </div>
                <div>
                    <a href="/#faq">FAQ</a>
                    <a href="/#sales">акции</a>
                </div>
                <div>
                    <NuxtLink to="/polytics">Политика конфиденциальности</NuxtLink>
                    <NuxtLink to="/terms">Пользовательское соглашение</NuxtLink>
                </div>
            </div>
        </div>
        <div class="footer__footer">
            <div>
                <small>г. Алматы, пр-кт Республики 13, Почтовый индекс A15X3C5 (050013), +7 775 816 82 48, БИН
                    210840016943</small>
                <small>Почта поддержки : support@caserust.kz</small>
            </div>
            <small>2023©rust.kz</small>
        </div>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            hideFooterOnPages: ['login', 'register'],
        }
    }
}
</script>
<style lang="scss" scoped>
footer {
    padding: 0 200px 70px;

    @media (max-width: 1600px) {
        padding: 0 50px 50px 200px;
    }

    @media (max-width: 1024px) {
        padding: 0 20px 37px;
    }

    .footer__footer {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        margin-top: 40px;

        @media (max-width: 1024px) {
            flex-direction: column;
            margin-top: 20px;
            gap: 15px;
        }

        small {
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #C8C8C8;
            display: block;
        }
    }

    .footer__body {
        margin-top: 50px;
        display: flex;
        gap: 6.406vw;
        align-items: flex-start;

        @media (max-width: 1600px) {
            gap: 50px;
        }

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 30px;
            margin-top: 30px;
        }

        img {
            @media (max-width: 1600px) {
                max-width: 100%;
            }

            @media (max-width: 1024px) {
                max-width: 165px;
            }
        }

        .links {
            display: flex;
            gap: 6.927vw;

            @media (max-width: 1660px) {
                gap: 50px;
            }

            @media (max-width: 1024px) {
                flex-direction: column;
                gap: 30px;
            }

            div {
                display: flex;
                flex-direction: column;
                gap: 29px;

                a {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--mon);
                    color: #fff;

                    @media (max-width: 1600px) {
                        font-size: 16px;
                    }
                }
            }
        }
    }

    hr {
        border-top: 2px solid #fff;
        border-radius: 10px;
        margin-top: 0;
    }
}
</style>