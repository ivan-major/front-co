<template>
    <div class="weather">
        <div class="weather__title">Temperature in cities of Ukraine</div>
        <div class="weather__container">
            <div class="weather__block date">
                <div class="date__header text text_header">
                    Choose a date (up to 7 days)
                </div>
                <button
                    class="date__button button text text_main text_button"
                    @click="openListDate"
                >
                    {{ time[date] }}
                    <div class="arrow" :class="isOpenDate && 'arrow_active'">
                        <svg
                            version="1.1"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 20 20"
                        >
                            <path
                                d="M2.582 13.891c-0.272 0.268-0.709 0.268-0.979 0s-0.271-0.701 0-0.969l7.908-7.83c0.27-0.268 0.707-0.268 0.979 0l7.908 7.83c0.27 0.268 0.27 0.701 0 0.969s-0.709 0.268-0.978 0l-7.42-7.141-7.418 7.141z"
                            ></path>
                        </svg>
                    </div>
                </button>
                <ul
                    class="date__list list"
                    :class="isOpenDate && 'list_active'"
                >
                    <li
                        class="date__item list__item text text_main"
                        v-for="(date, ind) in time"
                        :key="date"
                        @click="selectDate(ind)"
                    >
                        {{ date }}
                    </li>
                </ul>
            </div>

            <div class="weather__block cities">
                <div class="cities__header text text_header">
                    Add city which you remove
                </div>
                <button
                    class="cities__title button text text_main text_button"
                    :disabled="listAddCity.length > 0 ? false : true"
                    @click="openListCities"
                >
                    Add city
                    <div class="arrow" :class="isOpenCities && 'arrow_active'">
                        <svg
                            version="1.1"
                            xmlns="http://www.w3.org/2000/svg"
                            width="20"
                            height="20"
                            viewBox="0 0 20 20"
                        >
                            <path
                                d="M2.582 13.891c-0.272 0.268-0.709 0.268-0.979 0s-0.271-0.701 0-0.969l7.908-7.83c0.27-0.268 0.707-0.268 0.979 0l7.908 7.83c0.27 0.268 0.27 0.701 0 0.969s-0.709 0.268-0.978 0l-7.42-7.141-7.418 7.141z"
                            ></path>
                        </svg>
                    </div>
                </button>
                <ul
                    class="cities__list list"
                    :class="isOpenCities && 'list_active'"
                >
                    <li
                        class="cities__item list__item text text_main"
                        v-for="city in listAddCity"
                        :key="city.id"
                        @click="selectCity(city)"
                    >
                        {{ city.name }}
                    </li>
                </ul>
            </div>
        </div>

        <div class="weather__temp temp">
            <div class="temp__header">
                <div
                    class="temp__main text text_header"
                    @click="sortList('name')"
                >
                    City
                </div>
                <div
                    class="temp__main text text_header"
                    @click="sortList('min')"
                >
                    Min °C
                </div>
                <div
                    class="temp__main text text_header"
                    @click="sortList('max')"
                >
                    Max °C
                </div>
                <div class="temp__main text text_header"></div>
            </div>
            <ul class="temp__list">
                <li
                    class="temp__item item text text_table"
                    v-for="item in citiesWithTemp"
                    :key="item.id"
                >
                    <div class="item__info">{{ item.name }}</div>
                    <div class="item__info">
                        {{ item.daily.apparent_temperature_min[date] }}
                    </div>
                    <div class="item__info">
                        {{ item.daily.apparent_temperature_max[date] }}
                    </div>
                    <div class="item__info delete" @click="removeCity(item)">
                        <svg
                            version="1.1"
                            xmlns="http://www.w3.org/2000/svg"
                            width="32"
                            height="32"
                            viewBox="0 0 32 32"
                        >
                            <path
                                d="M30 24.398l-8.406-8.398 8.406-8.398-5.602-5.602-8.398 8.402-8.402-8.402-5.598 5.602 8.398 8.398-8.398 8.398 5.598 5.602 8.402-8.402 8.398 8.402z"
                                fill="#fff"
                            ></path>
                        </svg>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            date: 0,
            // city: {},
            cities: [
                {
                    id: 691650,
                    name: "Ternopil",
                    latitude: 49.55404,
                    longitude: 25.59067,
                },
                {
                    id: 693805,
                    name: "Simferopol",
                    latitude: 44.95719,
                    longitude: 34.11079,
                },
                {
                    id: 710791,
                    name: "Cherkassy",
                    latitude: 49.44452,
                    longitude: 32.05738,
                },
                {
                    id: 695594,
                    name: "Rivne",
                    latitude: 50.62308,
                    longitude: 26.22743,
                },
                {
                    id: 709717,
                    name: "Donetsk",
                    latitude: 48.023,
                    longitude: 37.80224,
                },
                {
                    id: 702658,
                    name: "Luhansk",
                    latitude: 48.56705,
                    longitude: 39.31706,
                },
                {
                    id: 687700,
                    name: "Zaporizhzhya",
                    latitude: 47.85167,
                    longitude: 35.11714,
                },
                {
                    id: 706448,
                    name: "Kherson",
                    latitude: 46.63695,
                    longitude: 32.61458,
                },
                {
                    id: 700569,
                    name: "Mykolaiv",
                    latitude: 46.97625,
                    longitude: 31.99296,
                },
                {
                    id: 696643,
                    name: "Poltava",
                    latitude: 49.58925,
                    longitude: 34.55367,
                },
                {
                    id: 706483,
                    name: "Kharkiv",
                    latitude: 49.98081,
                    longitude: 36.25272,
                },
                {
                    id: 692194,
                    name: "Sumy",
                    latitude: 50.9216,
                    longitude: 34.80029,
                },
                {
                    id: 686967,
                    name: "Zhytomyr",
                    latitude: 50.26487,
                    longitude: 28.67669,
                },
                {
                    id: 706369,
                    name: "Khmelnytskyy",
                    latitude: 49.41835,
                    longitude: 26.97936,
                },
                {
                    id: 689558,
                    name: "Vinnytsia",
                    latitude: 49.2322,
                    longitude: 28.46871,
                },
                {
                    id: 702569,
                    name: "Lutsk",
                    latitude: 50.75932,
                    longitude: 25.34244,
                },
                {
                    id: 702550,
                    name: "Lviv",
                    latitude: 49.83826,
                    longitude: 24.02324,
                },
                {
                    id: 707471,
                    name: "Ivano-Frankivsk",
                    latitude: 48.92312,
                    longitude: 24.71248,
                },
                {
                    id: 710735,
                    name: "Chernihiv",
                    latitude: 51.50551,
                    longitude: 31.28487,
                },
                {
                    id: 710719,
                    name: "Chernivtsi",
                    latitude: 48.29045,
                    longitude: 25.93241,
                },
                {
                    id: 690548,
                    name: "Uzhhorod",
                    latitude: 48.6242,
                    longitude: 22.2947,
                },
                {
                    id: 698740,
                    name: "Odessa",
                    latitude: 46.48572,
                    longitude: 30.74383,
                },
                {
                    id: 709930,
                    name: "Dnipro",
                    latitude: 48.46664,
                    longitude: 35.04066,
                },
                {
                    id: 703448,
                    name: "Kyiv",
                    latitude: 50.45466,
                    longitude: 30.5238,
                },
                {
                    id: 705812,
                    name: "Elizabethgrad",
                    latitude: 48.50834,
                    longitude: 32.26618,
                },
            ],
            citiesWithTemp: [],
            time: [],
            delCity: "",
            listAddCity: [],
            isOpenDate: false,
            isOpenCities: false,
            sortName: false,
            sortMin: false,
            sortMax: false,
        };
    },
    components: {},
    methods: {
        request(latitude, longitude) {
            return fetch(
                `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&daily=apparent_temperature_max,apparent_temperature_min&timezone=Europe%2FBerlin`
            ).then((response) => {
                const data = response.json();

                if (!response.ok) {
                    throw `${response.status} - ${response.statusText}`;
                }

                return data;
            });
        },

        removeCity(city) {
            this.citiesWithTemp = this.citiesWithTemp.filter(
                (item) => item.id !== city.id
            );

            this.listAddCity.push(city);
        },

        openListDate() {
            this.isOpenDate = !this.isOpenDate;
        },

        openListCities() {
            this.isOpenCities = !this.isOpenCities;
            this.listAddCity.sort((a, b) => a.name.localCompare(b.name));
        },

        selectDate(ind) {
            this.date = ind;
            this.isOpenDate = !this.isOpenDate;
        },

        selectCity(city) {
            this.isOpenCities = !this.isOpenCities;
            this.listAddCity = this.listAddCity.filter(
                (item) => item.id !== city.id
            );
            this.citiesWithTemp.push(city);
        },

        sortList(item) {
            if (item === "name") {
                !this.sortName
                    ? this.citiesWithTemp.sort((a, b) =>
                          a.name.localeCompare(b.name)
                      )
                    : this.citiesWithTemp.sort((a, b) =>
                          b.name.localeCompare(a.name)
                      );

                this.sortName = !this.sortName;
            }

            if (item === "min") {
                !this.sortMin
                    ? this.citiesWithTemp.sort(
                          (a, b) =>
                              a.daily.apparent_temperature_min[this.date] -
                              b.daily.apparent_temperature_min[this.date]
                      )
                    : this.citiesWithTemp.sort(
                          (a, b) =>
                              b.daily.apparent_temperature_min[this.date] -
                              a.daily.apparent_temperature_min[this.date]
                      );

                this.sortMin = !this.sortMin;
            }

            if (item === "max") {
                !this.sortMax
                    ? this.citiesWithTemp.sort(
                          (a, b) =>
                              a.daily.apparent_temperature_max[this.date] -
                              b.daily.apparent_temperature_max[this.date]
                      )
                    : this.citiesWithTemp.sort(
                          (a, b) =>
                              b.daily.apparent_temperature_max[this.date] -
                              a.daily.apparent_temperature_max[this.date]
                      );

                this.sortMax = !this.sortMax;
            }
        },
    },
    created() {
        this.cities.forEach(async (city) => {
            const temp = await this.request(city.latitude, city.longitude);
            this.citiesWithTemp.push({ ...city, ...temp });
            this.time = temp.daily.time;
        });
    },
};
</script>

<style lang="scss" scoped>
@import "@/assets/css/global.scss";

.weather {
    padding: 20px;

    &__title {
        font-weight: 700;
        font-size: 28px;
        text-align: center;

        color: #0d87b8;
        text-transform: uppercase;

        margin-bottom: 20px;
    }

    &__block {
        display: grid;
        gap: 10px;

        margin-bottom: 20px;

        &::after {
            content: "";
            display: block;
            height: 2px;
            background-color: #0d87b8;
        }
    }

    &__temp {
        border: 1px solid #0d88b879;
        border-radius: 20px;
        box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    }
}

.temp {
    &__header {
        padding: 5px;
        background-color: #0d88b879;
        border-radius: 20px 20px 0 0;
        display: flex;
    }

    &__main {
        flex: 1;

        &:first-child {
            flex: 1.5;
        }

        &:last-child {
            flex: 0.5;
        }
    }

    &__list {
        padding: 10px 5px;
    }

    &__item {
        display: flex;
        align-items: center;
    }
}

.item {
    flex-wrap: wrap;

    &::after {
        content: "";
        display: block;
        width: 100%;
        height: 1px;
        margin: 5px 0;
        background-color: #0d88b85f;
    }

    &__info {
        flex: 1;

        &:first-child {
            flex: 1.5;
        }

        &:last-child {
            flex: 0.5;
        }
    }
}

.delete {
    height: 18px;

    display: flex;
    border: 1px solid #051d44;
    background-color: #051d44;
    border-radius: 10px;

    cursor: pointer;

    svg {
        width: 16px;
        height: 16px;

        margin: auto;
    }

    &:hover {
        background-color: #fff;

        svg {
            path {
                fill: #051d44;
            }
        }
    }
}

.button {
    padding: 10px 20px;
    border: none;
    border-radius: 20px;
    background-color: #fff700d2;

    display: flex;
    justify-content: space-between;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;

    cursor: pointer;

    &:hover {
        background-color: #fff;
        outline: 4px solid #fff700d2;
    }

    &:disabled {
        opacity: 0.8;
        background-color: #fff700d2;
        outline: none;
    }
}

.list {
    padding: 0 20px;
    max-height: 0;
    transition: max-height 0.5s;
    overflow-y: scroll;

    &__item {
        &::after {
            content: "";
            display: block;
            height: 1px;
            margin: 10px 0;
            background-color: #0d88b85f;
        }
    }

    &_active {
        transition: max-height 0.5s;
        max-height: 150px;
    }
}
.text {
    text-transform: uppercase;

    &_header {
        font-weight: 700;
        font-size: 20px;
        color: #0f3e85;
    }

    &_main {
        font-weight: 400;
        font-size: 20px;
        color: #051d44;
    }

    &_button {
        font-weight: 700;
    }

    &_table {
        text-transform: none;
        font-size: 18px;
        color: #051d44;
    }
}

.arrow {
    transform: rotate(0);
    transition-duration: 500ms;

    &_active {
        transform: rotate(180deg);
        transition-duration: 500ms;
    }
}

@include tabl {
    .weather {
        padding: 50px;

        &__title {
            font-size: 32px;
            margin-bottom: 30px;
        }

        &__block {
            gap: 20px;

            margin-bottom: 20px;
            flex: 1;
            margin-right: 40px;

            &:last-child {
                margin-right: 0;
            }

            &::after {
                display: none;
            }
        }

        &__container {
            display: flex;
            flex-wrap: wrap;
            align-items: flex-start;

            &::after {
                content: "";
                width: 100%;
                display: block;
                height: 2px;
                background-color: #0d87b8;
                margin-bottom: 30px;
            }
        }

        &__temp {
            box-shadow: rgba(0, 0, 0, 0.24) 0px 7px 12px;
        }
    }

    .temp {
        &__header {
            padding: 20px;
        }

        &__main {
            &:last-child {
                flex: 0.2;
            }
        }

        &__list {
            padding: 20px;
        }
    }

    .item {
        &::after {
            margin: 10px 0;
        }

        &__info {
            &:last-child {
                flex: 0.2;
            }
        }
    }

    .delete {
        height: 22px;
        border-radius: 10px;
    }

    .button {
        box-shadow: rgba(0, 0, 0, 0.24) 0px 7px 12px;
    }

    .text {
        &_header {
            font-size: 24px;
        }

        &_main {
            font-size: 24px;
        }

        &_table {
            font-size: 22px;
        }
    }
}

@include desc {
    .weather {
        width: 1000px;
        margin: 40px auto;
        padding: 0;

        &__title {
            font-weight: 700;
            font-size: 36px;

            margin-bottom: 40px;
        }

        &__block {
            gap: 25px;
        }

        &__container {
            &::after {
                margin-bottom: 40px;
            }
        }

        &__temp {
            border-radius: 30px;
            box-shadow: rgba(0, 0, 0, 0.24) 0px 11px 16px;
        }
    }

    .temp {
        &__header {
            padding: 30px;
            border-radius: 30px 30px 0 0;
        }

        &__list {
            padding: 30px;
        }
    }

    .item {
        &::after {
            margin: 20px 0;
        }
    }

    .delete {
        height: 26px;
        border-radius: 15px;

        svg {
            width: 18px;
            height: 18px;
        }
    }

    .button {
        padding: 15px 25px;
        border-radius: 25px;

        box-shadow: rgba(0, 0, 0, 0.24) 0px 15px 20px;
    }

    .list {
        padding: 0 30px;
    }
    .text {
        &_header {
            font-size: 28px;
        }

        &_main {
            font-size: 28px;
        }

        &_table {
            font-size: 26px;
        }
    }
}
</style>
