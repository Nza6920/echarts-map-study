<template>
    <div id="app">
        <div class="map-china">
            <Map :map-id="'map-china'"
                 :area-click="areaClick1"
                 :map-style="'height: 500px; width: 700px;'"
                 :area="'中国'"
                 :scatter-coord="scatterCoord1"
                 :scatter-data="scatterData1"/>
        </div>
        <div v-if="province.showProvince" class="map-province">
            <Map :map-id="'map-province'"
                 :area-click="areaClick2"
                 :map-style="'height: 500px; width: 700px;'"
                 :area="province.name"
                 :scatter-coord="scatterCoord2"
                 :scatter-data="scatterData2"/>
        </div>
    </div>
</template>

<script>
    import Map from './components/Map.vue'

    export default {
        name: 'App',
        components: {
            Map
        },
        data() {
            return {
                province: {
                    showProvince: true,
                    name: '四川',
                },
                scatterCoord1: [
                    {
                        name: '青海',
                        coord: [101.76, 36.65]
                    },
                    {
                        name: '四川',
                        coord: [104.10, 30.65],
                    },
                    {
                        name: '北京',
                        coord: [116.23, 40.22],
                    },
                    {
                        name: '云南',
                        coord: [102.82, 24.88]
                    }
                ],
                scatterData1: [ // 散点数据
                    {
                        name: '青海',
                        value: {
                            qcSum: 16,
                            expertSum: 5,
                            hospitalSum: 3
                        }
                    }, {
                        name: '四川',
                        value: {
                            qcSum: 56,
                            expertSum: 17,
                            hospitalSum: 6
                        }
                    }, {
                        name: '云南',
                        value: {
                            qcSum: 36,
                            expertSum: 7,
                            hospitalSum: 9
                        }
                    }, {
                        name: '湖北',
                        value: {
                            qcSum: 26,
                            expertSum: 9,
                            hospitalSum: 12
                        }
                    }
                ],
                scatterCoord2: [
                    {
                        name: '成都',
                        coord: [104.10, 30.65],
                    }
                ],
                scatterData2: [ // 散点数据
                    {
                        name: '成都',
                        value: {
                            qcSum: 16,
                            expertSum: 5,
                            hospitalSum: 3
                        }
                    }
                ]

            }
        },
        methods: {
            areaClick1(param) {
                const _this = this
                console.log('点击了' + param.name)
                _this.province.showProvince = false
                _this.province.name = param.name

                // 1秒后重新显示
                setTimeout(function () {
                    _this.province.showProvince = true
                }, 1000)
            },
            areaClick2(param) {
                console.log('点击了' + param.name)
            }
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 60px;
    }
    .map-china {
        display: inline-block;
        float: left;
        width: 50%;
        height: 100%;
    }
    .map-province {
        display: inline-block;
        float: right;
        width: 50%;
        height: 100%;
    }
</style>
