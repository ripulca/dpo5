<!-- форма ввода города -->
<template>
    <div class="px-1 pt-2">
        <label class="form-label">Город проживания</label>
        <div class="input-group">
            <input type="text" v-model="inputValue" :id="'input-' + name">
            <ul v-show="vkData.cities.length > 0">
                <li v-for="city in vkData.cities" :key="city.id" @click="selectCity(city.id)">
                    г. {{ city.title }}
                </li>
            </ul>
        </div>
    </div>
</template>
  
<script>
import { CONFIG } from "../../config";
import jsonp from "jsonp";
export default {
    name: "ResumeInputCity",
    props: {
        name: {
            type: String,
            required: true,
        },
        vkData: {
            type: Object,
            required: true
        },
    },
    emits: ['updated'],
    data() {
        return {
            value: '',
        }
    },
    mounted() {
        this.queryRussiaCountryId();
    },
    computed: {
        inputValue: {
            get() {
                return this.value;
            },
            set(newValue) {
                this.value = newValue;
                this.queryCitiesList(newValue);
                this.$emit('updated', this.name, newValue);
            }
        }
    },
    methods: {
        queryCitiesList(query) {
            this.queryRussiaCountryId();
            this
                .get('database.getCities', {
                    country_id: this.vkData.russiaId,
                    q: query.substr(0, 15),
                    need_all: 1,
                    count: 10
                }, (function (err, data) {
                    if (err) {
                        console.error(err);
                    } else if (data.error) {
                        console.error(data.error)
                    } else {
                        this.vkData.cities = data.response.items;
                    }
                }).bind(this));
        },
        queryRussiaCountryId() {
            if (this.vkData.russiaId !== undefined) {
                return;
            }
            this
                .get('database.getCountries', {
                    code: 'RU',
                    count: 1
                }, (function (err, data) {
                    if (err) {
                        console.error(err);
                    } else if (data.error) {
                        console.error(data.error)
                    } else {
                        this.vkData.russiaId = data.response.items[0].id;
                    }
                }).bind(this));
        },
        formatCityData(city) {
            let s = city.title;
            let additionalData = [];
            if (city.area) {
                additionalData.push(city.area);
            }
            if (city.region) {
                additionalData.push(city.region);
            }
            if (additionalData.length > 0) {
                s += ' (' + additionalData.join(', ') + ')';
            }
            return s;
        },
        selectCity(cityId) {
            let city = this.vkData.cities.find((city) => city.id === cityId);
            this.value = city.title;
            this.vkData.cities = [];
            this.vkData.selectedCity = city;
            this.$emit('updated', this.name, city.title);
        },
        get(method, queryParams, fn) {
            queryParams.access_token = CONFIG.vk.token;
            queryParams.v = CONFIG.vk.version;
            queryParams.lang = 'ru';

            let uri = CONFIG.vk.uri + method + '?' + this.makeQueryString(queryParams);
            jsonp(uri, null, fn);
        },
        makeQueryString(queryParams) {
            return Array.from(Object.entries(queryParams))
                .map(value => value[0] + '=' + encodeURIComponent(value[1]))
                .join('&');
        }
    }
}
</script>