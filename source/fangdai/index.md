---
title: BigHouse
date: 2017-10-18 18:14:20
permalink: fangdai
layout: false
lang: en
---
{% raw %}
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/jquery/1.9.1/jquery.min.js"></script>
<script src="https://cdn.staticfile.org/vue/2.6.11/vue.min.js"></script>
<link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/3.4.1/css/bootstrap.min.css">
<script src="https://cdn.staticfile.org/twitter-bootstrap/3.4.1/js/bootstrap.min.js"></script>
<script src="https://cdn.staticfile.org/mathjs/10.5.3/math.min.js"></script>
<div id="app">
    <button type="button" class="btn btn-default" style="z-index: 10000;position: fixed;left: 0;top: 0;" v-on:click="moreDetail = !moreDetail">
        {{ moreDetail ? 'less' : 'more' }}</button>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-3">
                <h2 class="text-center text-primary" style="margin: 3rem auto 2rem auto;">贷款压力计算器</h2>
                <form class="form-horizontal">
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="total">房屋总价/万元</label>
                        <div class="col-sm-8">
                            <input id="total" v-model="total" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="first-pay-percent">首付比例</label>
                        <div class="col-sm-8">
                            <input id="first-pay-percent" v-model="first_pay_percent" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="year1">商贷贷款额/万元</label>
                        <div class="col-sm-8">
                            <input id="sd_total" v-model="sd_total" class=" form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="year1">商贷贷款年限</label>
                        <div class="col-sm-8">
                            <input id="year1" v-model="year1" class=" form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="sd-rate">商贷年利率</label>
                        <div class="col-sm-8">
                            <input id="sd-rate" v-model="sd_rate" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label">商贷还款方式</label>
                        <div class="radio col-sm-8">
                            <label><input v-model="sd_pay_method" type="radio" value="1">等额本金</label>
                            <label><input v-model="sd_pay_method" type="radio" value="2">等额本息</label>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="sf-total">首付总额/万元</label>
                        <div class="col-sm-8">
                            <input id="sf-total" v-model="sf_total" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="cash">自有现金/万元</label>
                        <div class="col-sm-8">
                            <input id="cash" v-model="cash" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="sf-cash">首付自有支出/万元</label>
                        <div class="col-sm-8">
                            <input id="sf-cash" v-model="sf_cash" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="sfd-total">首付贷总额/万元</label>
                        <div class="col-sm-8">
                            <input id="sfd-total" v-model="sfd_total" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="year2">首付贷偿还年限</label>
                        <div class="col-sm-8">
                            <input id="year2" v-model="year2" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="sfd-rate">首付贷利率</label>
                        <div class="col-sm-8">
                            <input id="sfd-rate" v-model="sfd_rate" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label">首付贷还款方式</label>
                        <div class="radio col-sm-8">
                            <label><input v-model="sfd_pay_method" type="radio" value="1">等额本金</label>
                            <label><input v-model="sfd_pay_method" type="radio" value="2">等额本息</label>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="gjj-total">公积金贷款总额/万元</label>
                        <div class="col-sm-8">
                            <input id="gjj-total" v-model="gjj_total" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="gjj-rate">公积金贷款年利率</label>
                        <div class="col-sm-8">
                            <input id="gjj-rate" v-model="gjj_rate" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="month_in">月收入/元</label>
                        <div class="col-sm-8">
                            <input id="month_in" v-model="month_in" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="rent">房租/元</label>
                        <div class="col-sm-8">
                            <input id="rent" v-model="rent" class="form-control" type="text"/>
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-sm-4 control-label" for="other_pay">其他支出/元</label>
                        <div class="col-sm-8">
                            <input id="other_pay" v-model="other_pay" class="form-control" type="text"/>
                        </div>
                    </div>

                    <div>
                        <button type="button" style="width: 100%;" class="btn btn-primary" @click="submit">提交</button>
                    </div>
                </form>
            </div>
            <div class="col-md-9">
                <table class="table table-hover table-striped">
                    <tr>
                        <th>期数</th>
                        <th>月收入</th>
                        <th>公积金</th>
                        <th>商贷还款额</th>
                        <th>首付贷还款额</th>
                        <th>房租</th>
                        <th>其他支出</th>
                        <th>现金结余</th>
                        <th v-bind:class="{ hide: !moreDetail }">商贷本金</th>
                        <th v-bind:class="{ hide: !moreDetail }">商贷利息</th>
                        <th v-bind:class="{ hide: !moreDetail }">首付贷本金</th>
                        <th v-bind:class="{ hide: !moreDetail }">首付贷利息</th>
                        <th v-bind:class="{ hide: !moreDetail }">公积金还款额</th>
                        <th v-bind:class="{ hide: !moreDetail }">公积金本金</th>
                        <th v-bind:class="{ hide: !moreDetail }">公积金利息</th>
                        <th v-bind:class="{ hide: !moreDetail }">公积金结余</th>
                        <th v-bind:class="{ hide: !moreDetail }">商贷剩余本金</th>
                        <th v-bind:class="{ hide: !moreDetail }">首付贷剩余本金</th>
                    </tr>
                    <tr v-for="(item, index) in month">
                        <td>{{index+1}}</td>
                        <td class="text-info">{{month_in}}</td>
                        <td class="text-info">{{gjj_in}}</td>
                        <td class="text-warning">{{item.sd_huankuane}}</td>
                        <td class="text-warning">{{item.sfd_huankuane}}</td>
                        <td class="text-warning">{{rent}}</td>
                        <td class="text-warning">{{other_pay}}</td>
                        <td class="text-success">{{left(month_in, item.sd_huankuane, item.sfd_huankuane, rent, other_pay)}}</td>
                        <td v-bind:class="{ hide: !moreDetail }">{{item.sd_benjin}}</td>
                        <td v-bind:class="{ hide: !moreDetail }">{{item.sd_rate}}</td>
                        <td v-bind:class="{ hide: !moreDetail }">{{item.sfd_benjin}}</td>
                        <td v-bind:class="{ hide: !moreDetail }">{{item.sfd_rate}}</td>
                        <td v-bind:class="{ hide: !moreDetail }">-</td>
                        <td v-bind:class="{ hide: !moreDetail }">-</td>
                        <td v-bind:class="{ hide: !moreDetail }">-</td>
                        <td v-bind:class="{ hide: !moreDetail }">-</td>
                        <td v-bind:class="{ hide: !moreDetail }">{{item.sd_left_benjin}}</td>
                        <td v-bind:class="{ hide: !moreDetail }">{{item.sfd_left_benjin}}</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</div>
<script>
    let formatConf = {notation: 'fixed', precision: 2}
    var app = new Vue({
        el: '#app',
        data: {
            // 汇总
            month: [],
            month_in: 0,
            gjj_in: 0,
            // 商贷
            sdMonth: [],
            sd_pay_method: 1,
            sd_rate: 4.9,
            // 首付贷
            sfdMonth: [],
            sfd_rate: 4.8,
            sfd_pay_method: 1,
            // 房屋总价
            total: 100,
            year1: 30,
            year2: 5,
            first_pay_percent: 30,
            cash: 0,
            sf_cash: 0,
            // 公积金
            gjj_total: 0,
            gjj_rate: 3.9,
            // 房租
            rent: 0,
            // 其他支出
            other_pay: 0,
            moreDetail: false,
        },
        computed: {
            sf_total: function () {
                return math.format(math.chain(this.total).multiply(this.first_pay_percent).divide(100).done(), formatConf)
            },
            sfd_total: function () {
                return math.format(math.chain(this.total).multiply(math.divide(this.first_pay_percent, 100)).subtract(this.sf_cash).done(), formatConf)
            },
            sd_total: function () {
                tmp = math.chain(this.total).multiply(math.divide(100 - this.first_pay_percent, 100)).subtract(this.gjj_total).done()
                if (tmp <= 0) {
                    return 0
                } else {
                    return math.format(tmp, formatConf)
                }
            }
        },
        methods: {
            left: function(inner, huankuan1, huankuan2, rent, other_apy) {
                if (huankuan1 === undefined) {
                    huankuan1 = 0
                }
                if (huankuan2 === undefined) {
                    huankuan2 = 0
                }
                return math.format(math.chain(inner).subtract(huankuan1).subtract(huankuan2).subtract(rent).subtract(other_apy).done(), formatConf)
            },
            submit: function (event) {
                if (this.sd_pay_method == 1) {
                    this.sdMonth = equality(this.sd_total, this.year1, this.sd_rate)
                } else {
                    this.sdMonth = average(this.sd_total, this.year1, this.sd_rate)
                }
                if (this.sfd_pay_method == 1) {
                    this.sfdMonth = equality(this.sfd_total, this.year2, this.sfd_rate)
                } else {
                    this.sfdMonth = average(this.sfd_total, this.year2, this.sfd_rate)
                }
                this.month = []
                let i = 0
                while (true) {
                    if (i >= this.sfdMonth.length && i >= this.sdMonth.length) {
                        break
                    }
                    tmp = {}
                    if (i < this.sdMonth.length) {
                        tmp["sd_benjin"] = this.sdMonth[i].benjin
                        tmp["sd_rate"] = this.sdMonth[i].rate
                        tmp["sd_huankuane"] = this.sdMonth[i].huankuane
                        tmp["sd_left_benjin"] = this.sdMonth[i].left_benjin
                    }
                    if (i < this.sfdMonth.length) {
                        tmp["sfd_benjin"] = this.sfdMonth[i].benjin
                        tmp["sfd_rate"] = this.sfdMonth[i].rate
                        tmp["sfd_huankuane"] = this.sfdMonth[i].huankuane
                        tmp["sfd_left_benjin"] = this.sfdMonth[i].left_benjin
                    }
                    this.month.push(tmp)
                    i++
                }
            }
        }
    })

    // 等额本金
    function equality(total, year, rateYear) {
        math.config({
            number: 'number',
            precision: 2,
        })
        if (total < 0) {
            alert("total invalid")
            return [];
        }
        if (year <= 0) {
            alert("year invalid")
            return [];
        }
        let month = math.floor(year * 12)
        let rateMonth = math.divide(math.divide(rateYear, 12), 100)
        total = total * 10000

        var payed = 0
        let res = []
        for (let i = 0; i < month; i++) {
            let benjin = math.divide(total, month)
            let rate = math.multiply(math.subtract(total, payed), rateMonth)
            let huankuane = math.add(benjin, rate)
            payed = math.add(payed, benjin)
            let left_benjin = math.subtract(total, payed)
            res.push({
                "benjin": math.format(benjin, formatConf),
                "rate": math.format(rate, formatConf),
                "huankuane": math.format(huankuane, formatConf),
                "left_benjin": math.format(left_benjin, formatConf),
            })
        }

        return res
    }

    // 等额本息
    function average(total, year, rateYear) {
        if (total <= 0) {
            alert("total invalid")
            return []
        }
        if (year <= 0) {
            alert("month invalid")
        }
        let res = []
        let payed = 0
        let month = math.floor(year * 12)
        let rateMonth = math.divide(math.divide(rateYear, 12), 100)
        total = total * 10000
        // A * β * (1 + β) ^ k / [(1 + β) ^ k - 1]
        huankuaneConst = math.divide(math.multiply(total, rateMonth, math.pow(1 + rateMonth, month)), math.pow(rateMonth + 1, month) - 1)
        firstMonthBenjin = math.subtract(huankuaneConst, math.multiply(total, rateMonth))
        for (let i = 1; i <= month; i++) {
            let rate, benjin
            if (i === 1) {
                benjin = firstMonthBenjin
            } else if (i === month) {
                benjin = math.subtract(total, payed)
            } else {
                benjin = math.multiply(firstMonthBenjin, math.pow(rateMonth + 1, i - 1))
            }
            rate = huankuaneConst - benjin
            payed = math.add(payed, benjin)
            let left_benjin = math.subtract(total, payed)
            res.push({
                "benjin": math.format(benjin, formatConf),
                "rate": math.format(rate, formatConf),
                "huankuane": math.format(huankuaneConst, formatConf),
                "left_benjin": math.format(left_benjin, formatConf),
            })
        }

        return res
    }
</script>
{% endraw %}