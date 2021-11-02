<template>
    <div class="delivery">
        <label>
            Населенний пункт
            <select name="city" v-model="selected.cityRef" @click="loadSettlements">
                <option v-for="city in cities" :key="city.AreasCenter" :value="city.AreasCenter">{{ city.Description }}</option>
            </select>
        </label>
        <label v-if="selected.cityRef && settlements">
            Поштове відділення
            <select name="city" v-model="selected.SettlementRef">
                <option v-for="(settlement, index) in settlements" :key="index" :value="settlement.SettlementRef">{{ settlement.Description }}</option>
            </select>
        </label>
    </div>
</template>

<script>
  export default {
    name: 'Delivery',
    data() {
        return {
			cities: [],
            settlements: [],
            selected: {
                cityRef: '',
                settlementRef: '',
            }
		}
    },
    mounted() {
        fetch('https://api.novaposhta.ua/v2.0/json/', {
            method: 'POST',
            body: JSON.stringify({
                apiKey: "f64594b8631e5da9f8de03b979ca607d",
                modelName: "Address",
                calledMethod: "getAreas",
                methodProperties: {}
            })
        })
        .then((response) => response.json())
        .then((data) => this.cities = data.data)
    },
    methods: {
        loadSettlements() {
            fetch('https://api.novaposhta.ua/v2.0/json/', {
            method: 'POST',
            body: JSON.stringify({
                apiKey: "f64594b8631e5da9f8de03b979ca607d",
                modelName: "AddressGeneral",
                calledMethod: "getWarehouses",
                methodProperties: {
                    CityRef: this.selected.cityRef,
                    Language: "ua"
                },
            })
        })
        .then((response) => response.json())
        .then((data) => this.settlements = data.data)
        }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
    label {
        display: block;
        font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
        font-size: 24px;
        margin-bottom: 36px;
    }
    select {
        display: block;
        margin: 12px auto;
        font-size: 24px;
        padding: 12px;
        width: 700px;
    }
</style>
