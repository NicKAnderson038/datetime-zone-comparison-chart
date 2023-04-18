<script>
    import { beforeUpdate } from 'svelte'
    // @ts-ignore
    import { chart } from 'svelte-apexcharts'
    import { isNumber, isUndefined } from 'is-what'
    import moment from 'moment'
    import MediaQuery from './MediaQuery.svelte'

    // export let timeZone = 'America/Chicago'
    export let start
    export let end
    export let timeZone1
    export let timeZone2

    function getTimezoneOffset(num) {
        num = Number(num.split(':')[0])
        const offset = new Date().getTimezoneOffset()
        const sign = offset > 0 ? '+' : '-'
        const value = Math.abs(num + Number(`${sign}${Math.abs(offset) / 60}`))
        return value.toString().length == 1 ? `0${value}` : `${value}`
    }

    const date = new Date()
    const year = date.getFullYear()
    // console.log(year) // 👉️ 2025
    const month = String(date.getMonth() + 1).padStart(2, '0')
    // console.log(month) // 👉️ 03
    const day = String(date.getDate()).padStart(2, '0')
    // console.log(day) // 👉️ 05

    let START
    let END
    let dataStore = []
    // const START = `${year}-${month}-${day}T${getTimezoneOffset(start)}:00:00.000Z`
    // const END = `${year}-${month}-${day}T${getTimezoneOffset(end)}:00:00.000Z`
    // const START_02 = '2023-04-18T16:00:00.000Z'
    // const END_02 = '2023-04-18T24:00:00.000Z'

    const myTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone

    function timeZoneTool(d, timeZone) {
        const date = new Date(d).toLocaleString('en-US', {
            timeZone,
        })
        return new Date(date).getTime()
    }

    let options = {
        chart: {
            height: 350,
            type: 'rangeBar',
        },
        tooltip: {
            // fixed: {
            //       enabled: true,
            //       position: 'topLeft', // topRight, topLeft, bottomRight, bottomLeft
            //       offsetY: 30,
            //       offsetX: 60
            //     },
            // items: {
            //     display: 'flex',
            // },
            x: {
                show: true,
                formatter: (val, obj) => {
                    const numberDate = (val, format) =>
                        moment(val).format(format)
                    return isNumber(val) ? numberDate(val, 'ddd hh A') : val
                },
            },
            style: {
                fontSize: '12px',
            },
        },
        series: [
            {
                data: [
                    // {
                    //     x: myTimezone.replace(/.*\//, '').replaceAll('_', ' '),
                    //     y: [new Date(START).getTime(), new Date(END).getTime()],
                    //     fillColor: '#008FFB',
                    // },
                    // {
                    //     x: timeZone.replace(/.*\//, ''),
                    //     y: [timeZoneTool(START, timeZone), timeZoneTool(END, timeZone)],
                    //     fillColor: '#00E396',
                    // },
                    // {
                    //     x: 'Coding',
                    //     y: [
                    //         new Date('2023-04-19T16:00:00.000Z').getTime(),
                    //         new Date('2023-04-19T24:00:00.000Z').getTime(),
                    //     ],
                    //     fillColor: '#775DD0',
                    // },
                    // {
                    //     x: 'Testing',
                    //     y: [
                    //         new Date('2019-03-08').getTime(),
                    //         new Date('2019-03-12').getTime(),
                    //     ],
                    //     fillColor: '#FEB019',
                    // },
                    // {
                    //     x: 'Deployment',
                    //     y: [
                    //         new Date('2019-03-12').getTime(),
                    //         new Date('2019-03-17').getTime(),
                    //     ],
                    //     fillColor: '#FF4560',
                    // },
                ],
            },
        ],
        plotOptions: {
            bar: {
                horizontal: true,
                distributed: true,
                dataLabels: {
                    hideOverflowingLabels: false,
                },
                style: {
                    colors: [],
                    fontSize: '6px',
                    fontFamily: 'Roboto',
                    fontWeight: 80,
                    //cssClass: 'apexcharts-xaxis-label',
                },
            },
        },
        dataLabels: {
            enabled: true,
            formatter: function (val, opts) {
                // @ts-ignore
                // const label = opts.w.globals.labels[opts.dataPointIndex]
                // const a = moment(val[0]).format('ddd hh A')
                // let b = moment(val[1]).format('ddd hh A')
                // if (
                //     moment(val[0]).format('ddd') ===
                //     moment(val[1]).format('ddd')
                // ) {
                //     b = moment(val[1]).format('hh A')
                // }

                const a = moment(val[0]).format('hh A')
                const b = moment(val[1]).format('hh A')
                // const diff = b.diff(a, 'hours')
                // return label + ': ' + diff + (diff > 1 ? ' hours' : ' hour')
                return `  ${a} - ${b}  `
            },
            style: {
                colors: ['#f3f4f5', '#fff'],
            },
        },
        // responsive: [
        //     {
        //         breakpoint: 1000,
        //         // options: {
        //         //     plotOptions: {
        //         //         bar: {
        //         //             horizontal: false,
        //         //         },
        //         //     },
        //         //     legend: {
        //         //         position: 'bottom',
        //         //     },
        //         // },
        //     },
        // ],
        // fill: {
        //   type: 'gradient',
        //   gradient: {
        //     shadeIntensity: 1,
        //     opacityFrom: 0.7,
        //     opacityTo: 0.9,
        //     stops: [0, 100]
        //   }
        // },
        // xaxis: {
        //     type: 'datetime',
        //     labels: {
        //         formatter: value => {
        //             return moment(value).format('ddd hh A')
        //         },
        //     },
        // },
        grid: {
            row: {
                colors: ['#f3f4f5', '#fff'],
                opacity: 1,
            },
        },
    }

    function getRanges(date, calc) {
        const d = new Date(date)
        const res = d.setDate(d.getDate() + calc)
        return new Date(res).toISOString()
    }

    beforeUpdate(() => {
        START = `${year}-${month}-${day}T${getTimezoneOffset(start)}:00:00.000Z`
        END = `${year}-${month}-${day}T${getTimezoneOffset(end)}:00:00.000Z`

        dataStore = [
            {
                data: [
                    {
                        x: timeZone1?.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(getRanges(START, -1), timeZone1),
                            timeZoneTool(getRanges(END, -1), timeZone1),
                        ],
                        fillColor: '#169bff',
                    },
                    {
                        x: timeZone2.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(getRanges(START, -1), timeZone2),
                            timeZoneTool(getRanges(END, -1), timeZone2),
                        ],
                        fillColor: '#00e496',
                    },
                ],
            },
            {
                data: [
                    {
                        x: timeZone1?.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(START, timeZone1),
                            timeZoneTool(END, timeZone1),
                        ],
                        fillColor: '#0080e2',
                    },
                    {
                        x: timeZone2?.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(START, timeZone2),
                            timeZoneTool(END, timeZone2),
                        ],
                        fillColor: '#00ca85',
                    },
                ],
            },
            {
                data: [
                    {
                        x: timeZone1?.replace(/.*\//, ''),
                        // x: myTimezone.replace(/.*\//, '').replaceAll('_', ' '),
                        y: [
                            timeZoneTool(getRanges(START, 1), timeZone1),
                            timeZoneTool(getRanges(END, 1), timeZone1),
                        ],
                        fillColor: '#169bff',
                    },
                    {
                        x: timeZone2.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(getRanges(START, 1), timeZone2),
                            timeZoneTool(getRanges(END, 1), timeZone2),
                        ],
                        fillColor: '#00e496',
                    },
                ],
            },
        ]

        options.series = dataStore
    })

    function multSeries(options) {
        options.series = dataStore
        options.xaxis = {
            type: 'datetime',
            labels: {
                formatter: value => {
                    return moment(value).format('ddd hh A')
                },
            },
        }
        return options
    }

    function singleSeries(options) {
        options.series = [dataStore[1]]
        options.xaxis = {
            type: 'datetime',
            labels: {
                formatter: value => {
                    return moment(value).format('hh A')
                },
            },
        }
        return options
    }
</script>

<div class="chart">
    <MediaQuery query="(min-width: 481px)" let:matches>
        {#if matches}
            <div use:chart={multSeries(options)} />
        {/if}
    </MediaQuery>
    <MediaQuery query="(max-width: 480px)" let:matches>
        {#if matches}
            <div use:chart={singleSeries(options)} />
        {/if}
    </MediaQuery>
</div>

<style>
    .chart {
        width: 100%;
    }
</style>
