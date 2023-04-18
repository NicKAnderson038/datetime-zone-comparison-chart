<script>
    import { beforeUpdate } from 'svelte'
    // @ts-ignore
    import { chart } from 'svelte-apexcharts'
    import { isNumber } from 'is-what'
    import moment from 'moment'

    function getTimezoneOffset(num) {
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

    const START = `${year}-${month}-${day}T${getTimezoneOffset(9)}:00:00.000Z`
    const END = `${year}-${month}-${day}T${getTimezoneOffset(17)}:00:00.000Z`
    // const START_02 = '2023-04-18T16:00:00.000Z'
    // const END_02 = '2023-04-18T24:00:00.000Z'

    // export let timeZone = 'America/Chicago'
    export let timeZone1
    export let timeZone2
    const myTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone

    function timeZoneTool(d, timeZone) {
        // console.log(d, timeZone)

        const date = new Date(d).toLocaleString('en-US', {
            timeZone,
        })
        console.log(timeZone, d, date)
        // const x = new Date(date).getTime()
        // console.log(x)
        // const y = new Intl.DateTimeFormat('en-US', { dateStyle: 'full', timeStyle: 'long', timeZone }).format(x)
        // console.log(timeZone, y, new Date(y).getTime());
        return new Date(date).getTime()
    }

    let options = {
        chart: {
            height: 350,
            type: 'rangeBar',
        },
        tooltip: {
            x: {
                show: true,
                formatter: val => {
                    const numberDate = val => moment(val).format('dddd hh A')
                    return isNumber(val) ? numberDate(val) : val
                },
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
                const label = opts.w.globals.labels[opts.dataPointIndex]
                const a = moment(val[0]).format('ddd hh A')
                let b = moment(val[1]).format('ddd hh A')
                if (
                    moment(val[0]).format('ddd') ===
                    moment(val[1]).format('ddd')
                ) {
                    b = moment(val[1]).format('hh A')
                }
                // const diff = b.diff(a, 'hours')
                // return label + ': ' + diff + (diff > 1 ? ' hours' : ' hour')
                return `  ${a} - ${b}  `
            },
            style: {
                colors: ['#f3f4f5', '#fff'],
            },
        },
        xaxis: {
            labels: {
                formatter: (value, timestamp) => {
                    // return new Date(timestamp) // The formatter function overrides format property
                    return moment(value).format('ddd')
                },
            },
        },
        // yaxis: {
        //     show: false,
        // },
        grid: {
            row: {
                colors: ['#f3f4f5', '#fff'],
                opacity: 1,
            },
        },
    }

    beforeUpdate(() => {
        // options.series[0].data = [
        //     {
        //         x: myTimezone.replace(/.*\//, '').replaceAll('_', ' '),
        //         y: [new Date(START).getTime(), new Date(END).getTime()],
        //         fillColor: '#008FFB',
        //     },
        //     {
        //         x: timeZone.replace(/.*\//, ''),
        //         y: [timeZoneTool(START, timeZone), timeZoneTool(END, timeZone)],
        //         fillColor: '#00E396',
        //     },
        // ]
        options.series = [
            {
                data: [
                    // {
                    //     x: timeZone1?.replace(/.*\//, ''),
                    //     y: [
                    //         new Date(START).getTime(),
                    //         new Date(END).getTime(),
                    //     ],
                    //     fillColor: '#008FFB',
                    // },
                    {
                        x: timeZone1?.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(START, timeZone1),
                            timeZoneTool(END, timeZone1),
                        ],
                        fillColor: '#008FFB',
                    },
                    {
                        x: timeZone2?.replace(/.*\//, ''),
                        y: [
                            timeZoneTool(START, timeZone2),
                            timeZoneTool(END, timeZone2),
                        ],
                        fillColor: '#00E396',
                    },
                ],
            },
            // {
            //     data: [
            //         {
            //             x: myTimezone.replace(/.*\//, '').replaceAll('_', ' '),
            //             y: [
            //                 new Date(START_02).getTime(),
            //                 new Date(END_02).getTime(),
            //             ],
            //             fillColor: '#008FFB',
            //         },
            //         {
            //             x: timeZone.replace(/.*\//, ''),
            //             y: [
            //                 timeZoneTool(START_02, timeZone),
            //                 timeZoneTool(END_02, timeZone),
            //             ],
            //             fillColor: '#00E396',
            //         },
            //     ],
            // },
        ]
    })
</script>

<div class="chart">
    <div use:chart={options} />
</div>

<style>
    .chart {
        width: 100%;
    }
</style>
