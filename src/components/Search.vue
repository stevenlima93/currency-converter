<template>

       <ion-item>
           <ion-label color="primary" stacked>Amount</ion-label>
           <ion-input placeholder="EUR" type="number" v-model="deviseToConvert"></ion-input>
       </ion-item>

       <ion-item>
           <ion-label>Devises</ion-label>
           <ion-select ok-text="OK" cancel-text="Cancel">
               <ion-select-option v-for="(devise, index) in devises"  @click="deviseSelected= index" :key="index" :value="index">{{ index }} : {{ devise }}</ion-select-option>
               </ion-select>
       </ion-item>

       <ion-button @click="convert">test</ion-button>


</template>

<script>
    import { IonLabel, IonInput, IonItem } from '@ionic/vue';
    import { defineComponent } from 'vue';
    import mitt from 'mitt'

    export default defineComponent({
        props: ['devises', 'rates'],
        data() {
            return {
                deviseToConvert: '',
                deviseSelected: '',
                deviseSelectedTmp: ''
            }
        },
        methods: {
            convert() {
                const emitter = mitt()
                let result = this.deviseToConvert * this.rates[this.deviseSelected]
                this.result = result
                this.deviseSelectedTmp = this.deviseSelected
                emitter.emit('result', {result: this.result, deviseSelectedTmp: this.deviseSelectedTmp, deviseSelected: this.deviseSelected, deviseToConvert: this.deviseToConvert})
                this.deviseToConvert = ''
            }
        },
        components: { IonLabel, IonInput, IonItem }
    });
</script>


