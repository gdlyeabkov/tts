<template>
    <div class="footer">
        <div class="footerImgContainer">
            <div class="likeContainer">
                <button class="btn btn-primary likeBtn">
                    <span class="material-icons likeBtnItem">
                        thumb_up
                    </span>
                    <span class="likeBtnItem">
                        Лайков
                    </span>
                    <span class="likeBtnItem">
                        6K
                    </span>
                </button>
                <button class="btn btn-primary likeBtn">
                    <span class="likeBtnItem">
                        Поделиться
                    </span>
                </button>
            </div>
            <div class="footerImgContainerItem">
                <img src="https://texttospeechrobot.com/tts/ru/sintezator-rechi-onlayn/images/sintezator-rechi-onlayn-v-russkom.png" alt="" width="250px" />
                <button class="btn btn-danger w-100">
                    Подписаться
                </button>
            </div>
            <span class="footerImgContainerItem">
                Copyright 2016 logoTTS roBOT
            </span>
        </div>
    </div>
</template>

<script>
export default {

}
</script>

<style scoped>
    
    .footer {
        background-color: rgb(230, 230, 230);
        padding: 50px 0px;
    }

    .footerImgContainer {
        display: flex;
        align-items: center;
        flex-direction: column;
    }

    .footerImgContainerItem {
        width: 250px;
        margin: 35px 0px;
    }

    .likeContainer {
        box-sizing: border-box;
        padding: 50px 20px;
        display: flex;
        width: 65%;
        border-radius: 8px;
        box-shadow: 0px 0px 2px rgb(150, 150, 150);
        height: 150px;
        background-color: rgb(255, 255, 255);
    }

    .likeBtn {
        height: 35px;
        margin: 0px 15px;
        display: flex;
        align-items: center;
    }

    .likeBtnItem {
        margin: 0px 5px;
    }

</style>