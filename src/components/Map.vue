<template>
    <div :id="mapId" :style="mapStyle">

    </div>
</template>

<script>
    export default {
        name: 'Map',
        props: {
            mapId: {
              type: String,
              default: function () {
                  return 'map'
              }
            },
            mapStyle: {
                type: String,
                default: function () {
                    return 'height: 800px; width: 1000px;'
                }
            },
            area: {
                type: String,
                default: function () {
                    return '中国'
                }
            },
            scatterCoord: {
                type: Array,
                default: function () {
                    return [
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
                    ]
                }
            },
            scatterData: {
                type: Array,
                default: function () {
                    return [ // 散点数据
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
                    ]
                }
            },
            convertData: {
                type: Function,
                default: function(data) { // 处理数据函数
                    const res = []
                    for (let i = 0; i < data.length; i++) {
                        for (let j = 0; j < this.scatterCoord.length; j++) {
                            const geoCoord = this.scatterCoord[j]
                            if (geoCoord.name === data[i].name) {
                                res.push({
                                    name: data[i].name,
                                    value: geoCoord.coord.concat(data[i].value)
                                })
                                break;
                            }
                        }
                    }
                    return res
                }
            },
            areaClick: {
                type: Function,
                default: function(param) {
                    console.log('点击了' + param.name)
                }
            }
        },
        data() {
            return {
                provinces: ['shanghai', 'hebei', 'shanxi', 'neimenggu', 'liaoning', 'jilin', 'heilongjiang', 'jiangsu', 'zhejiang', 'anhui', 'fujian', 'jiangxi', 'shandong', 'henan', 'hubei', 'hunan', 'guangdong', 'guangxi', 'hainan', 'sichuan', 'guizhou', 'yunnan', 'xizang', 'shanxi1', 'gansu', 'qinghai', 'ningxia', 'xinjiang', 'beijing', 'tianjin', 'chongqing', 'xianggang', 'aomen', 'taiwan'],
                provincesText: ['上海', '河北', '山西', '内蒙古', '辽宁', '吉林', '黑龙江', '江苏', '浙江', '安徽', '福建', '江西', '山东', '河南', '湖北', '湖南', '广东', '广西', '海南', '四川', '贵州', '云南', '西藏', '陕西', '甘肃', '青海', '宁夏', '新疆', '北京', '天津', '重庆', '香港', '澳门', '台湾'],

                options: {
                    backgroundColor: '#fff',
                    tooltip: {
                        trigger: 'item',

                    },
                    geo: {
                        // map: 'china' | '四川'
                        // 必须要先引入了对应地图的js文件或者json文件，在这一步的时候，echarts会自动将对应的JS文件注入，地图才会显示.
                        map: this.area,
                        roam: false,//不开启缩放和平移
                        zoom: 1.23,//视角缩放比例
                        label: {
                            normal: {
                                show: true,
                                fontSize: '10',
                                color: '#ADB4A9'
                            },
                        },
                        itemStyle: {
                            emphasis: {
                                areaColor: '#5DBBEB',//鼠标选择区域颜色
                                shadowOffsetX: 0,
                                shadowOffsetY: 0,
                                shadowBlur: 10,
                                borderWidth: 0,
                                shadowColor: '#888888'
                            },
                            normal: {
                                areaColor: '#D9EFCA',// 地图区域颜色
                                borderColor: '#FFFFFF',
                                borderWidth: 1,
                                shadowBlur: 10,
                                shadowColor: '#cee2c2',
                                shadowOffsetX: 1,
                                shadowOffsetY: 3  ,
                            }
                        }
                    },
                    series: [
                        { // 散点配置
                            name: '数量',
                            type: 'scatter',
                            coordinateSystem: 'geo',
                            data: this.convertData(this.scatterData),
                            symbolSize: function (val) {
                                const info = val[2]
                                return (info.expertSum + info.hospitalSum + info.qcSum) / 2
                            },
                            hoverAnimation: true,
                            itemStyle: {
                                color: '#90CBD2',
                                borderWidth: 1,
                                borderColor: '#5DBBEB'
                            },
                            tooltip: {
                                formatter: function (params) {
                                    const province = params.data.name + '省'
                                    const info = params.data.value[2];
                                    const res = []
                                    res.push(`<span style="display: inline-block; width: 100%; text-align: left; letter-spacing: 1px">${province}</span><br>`)
                                    res.push(`<span style="display: inline-block; width: 100%; text-align: left; letter-spacing: 1px">xx人数 : <span style="letter-spacing: normal;font-size: 15px">${info.qcSum}</span> <span style="float: right; padding-left: 3px">人</span></span><br>`)
                                    res.push(`<span style="display: inline-block; width: 100%; text-align: left; letter-spacing: 1px">xx数量 : <span style="letter-spacing: normal;font-size: 15px">${info.expertSum}</span> <span style="float: right; padding-left: 3px">人</span></span><br>`)
                                    res.push(`<span style="display: inline-block; width: 100%; text-align: left; letter-spacing: 1px">xx数量 : <span style="letter-spacing: normal;font-size: 15px">${info.hospitalSum}</span> <span style="float: right; padding-left: 3px">家</span></span><br>`)
                                    return res.join('')
                                },
                                textStyle: {
                                    fontStyle: 'normal',
                                    fontSize: 13,
                                    lineHeight: 60
                                }
                            }
                        }
                    ]
                }
            }
        },
        mounted() {
            const _this = this
            this.$nextTick(function () {
                _this.initMap(_this.area)
            })
        },
        methods: {
            initMap(param) {

                // 引入地图
                if (param === '中国') {
                    param = 'china'
                } else {
                    param = 'province/' + this.provinces[this.provincesText.indexOf(param)]
                }
                console.log(param)
                require(`../assets/map/js/${param}`)

                const map = this.$echarts.init(document.getElementById(this.mapId));
                map.setOption(this.options);
                map.on('click', this.areaClick)
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css">

</style>
