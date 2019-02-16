<template>
    <div class="stock-viewer">
        <h1>
            {{ quote.symbol }}
            <span class="change">
                <span v-if="getPercentChange() > 0" class="green number">
                    &#x2191; {{ getPercentChange() }} %
                </span>
                <span v-else-if="getPercentChange() < 0" class="red number">
                    &#x2193; {{ getPercentChange() }} %
                </span>
                <span v-else class="number">
                    {{ getPercentChange() }} %
                </span>
            </span>
        </h1>
        <div class="subtitle"> {{ quote.companyName }} </div>
        <div class="data-container">
            <div class="left-column">
                <div class="last-price data-row">
                    <span>Last Price</span>
                    <span class="number">${{ getFixedPrice(quote.latestPrice) }} </span>
                </div>
                <div class="open-price  data-row">
                    <span>Open Price</span>
                    <span class="number">${{ getFixedPrice(quote.open) }} </span>
                </div>
                <div class="high-price  data-row">
                    <span>High Price</span>
                    <span class="number">${{ getFixedPrice(quote.high) }} </span>
                </div>
                <div class="low-price  data-row">
                    <span>Low Price</span>
                    <span class="number">${{ getFixedPrice(quote.low) }} </span>
                </div>
            </div>
            <div class="right-column">
                <div class="volume data-row">
                    <span>Total Volume</span>
                    <span class="number">{{ quote.avgTotalVolume }}</span>
                </div>
                <div class="year-to-date-change data-row">
                    <span>YTD Change</span>
                    <span v-if="getYTDChange() > 0" class="green">
                        &#x2191; {{ getYTDChange() }}%
                    </span>
                    <span v-else class="red">
                        &#x2193; {{ getYTDChange() }}%
                    </span>
                </div>
                <div class="dividend data-row">
                    <span>Dividend</span>
                    <span class="number">{{ getDividendAmount }}</span>
                </div>
                <div class="last-payout-date data-row">
                    <span>Div Date</span>
                    <span class="number">{{ getDividendPaymentDate }}</span>
                </div>
            </div>
        </div>
    </div>    
</template>
<script>
export default {
    props: [ 'info' ],
    data() {
        return {
            quote: this.info.data.quote,
            dividend: this.info.data.dividends[0],
        }
    },
    computed: {
        getDividendAmount() {
            var d = this.dividend
            if (d) {
                return "$" + d.amount;
            } else {
                return 'N/A'
            }
        },
        getDividendPaymentDate() {
            var d = this.dividend
            if (d) {
                return d.paymentDate;
            } else {
                return 'N/A'
            }
        }
    },
    methods: {
        getPercentChange() {
            return (this.quote.changePercent * 100).toFixed(2);
        },
        getYTDChange() {
            return (this.quote.ytdChange * 100).toFixed(2);
        },
        getFixedPrice(input) {
            return input.toFixed(2);
        }
    }
}
</script>
<style lang="scss">
.number {
    font-family: 'SF Mono', 'Roboto Mono', monospace;
}
h1 {
    margin: 10px 0 4px;
    padding: 0;
}
.stock-viewer {
    width: 346px;
    margin: auto;
}
.left-column, .right-column {
    display: flex;
    flex-direction: column;
    width: 142px;
}
.data-row {
    margin: 4px 0;
    letter-spacing: 0.01em;
    display: flex;
    justify-content: space-between;
}
.change {
    font-size: 18px;
}
.green {
    color: #6DA317;
}
.red {
    color: #E9110B;
}
.data-container {
    display: flex;
    justify-content: space-between;
    margin-top: 22px;
}
</style>
