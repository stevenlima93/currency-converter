<template>
    <ion-page>
        <ion-header :translucent="true">
            <ion-toolbar>
                <ion-title>Currency converter</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content :fullscreen="true">
            <ion-header collapse="condense">
                <ion-toolbar>
                    <ion-title size="large">Blank</ion-title>
                </ion-toolbar>
            </ion-header>

            <div id="container">
                <search :devises="devises" :rates="rates" @result="resultat"></search>
                <result :result="result" :deviseSelectedTmp="deviseSelectedTmp"></result>
            </div>
        </ion-content>
    </ion-page>
</template>

<script>
    import {IonContent, IonHeader, IonPage, IonTitle, IonToolbar} from '@ionic/vue';
    import {defineComponent} from 'vue';
    import Search from '@/components/Search.vue';
    import axios from 'axios';
    import mitt from 'mitt'

    export default defineComponent({
        name: 'Home',
        data() {
            return {
                devises: [],
                rates: [],
                deviseToConvert: '',
                deviseSelected: '',
                deviseSelectedTmp: '',
                result: ''
            }
        },
        mounted() {
            axios.get(`http://data.fixer.io/api/symbols?access_key=${process.env.VUE_APP_API_KEY}`).then(res => {
                this.devises = res.data.symbols
            })

            axios.get(`http://data.fixer.io/api/latest?access_key=${process.env.VUE_APP_API_KEY}`).then(res => {
                this.rates = res.data.rates
            })
        },
        created() {
            const emitter = mitt()

            emitter.on("result", data=>{
                console.log(data)
            })
        },
        methods: {
            resultat(result) {
                console.log(result)
                this.result = result.result
                this.deviseSelectedTmp = result.deviseSelectedTmp
                this.deviseSelected = result.deviseSelected
                this.deviseToConvert = result.deviseToConvert
            }
        },
        components: {
            IonContent,
            IonHeader,
            IonPage,
            IonTitle,
            IonToolbar,
            Search
        }
    });
</script>

<style scoped>
    #container {
        text-align: center;

        position: absolute;
        left: 0;
        right: 0;
        top: 50%;
        transform: translateY(-50%);
    }

    #container strong {
        font-size: 20px;
        line-height: 26px;
    }

    #container p {
        font-size: 16px;
        line-height: 22px;

        color: #8c8c8c;

        margin: 0;
    }

    #container a {
        text-decoration: none;
    }
</style>
