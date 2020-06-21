<template>
    <div id="indicator" >
        <div id="container" v-bind:class="compColor">

            <div id="rank">Rank: {{rank}} </div>

            <div id="UserPoints">
                <div id="points" >{{ points }} </div>
                <div id ="text" >points</div>
            </div>
        </div>

        <div id="detailLeft" v-bind:class="compColor"/>
        <div id="detailRight" v-bind:class="compColor"/>
        <div id="smallDetailLeft" v-bind:class="compColor"/>
        <div id="smallDetailRight" v-bind:class="compColor"/>
    </div>

</template>

<script>
    export default {
        name: 'RankPosition',
        props: {
            rank: Number,
            points: Number,
            position: String
        },
        computed: {
            compColor: function() {
                let first = false, last = false, middle = false;
                if (this.position === "first") {
                    first = true;
                } else if (this.position === "last") {
                    last = true;
                } else {
                    middle = true;
                }
                return {
                    first: first,
                    last: last,
                    middle: middle
                }
            }
        }
    }
</script>

<style scoped>
    #indicator{
        margin-top: 20px;
        height: 200px;
        width: 600px;
        margin-left: auto;
        margin-right: auto;
        background: none;
    }
    .first {
        background: #E06E3C;
    }
    .middle {
        background: #28847B;
    }
    .last {
        background: #808080;
    }
    #container{
        margin-left: auto;
        margin-right: auto;
        display: table;
        width: 200px;
        height: 200px;
        border-radius: 100%;
    }

    #rank{
        position: absolute;
        font-family:Verdana;
        color: white;
        font-size: 30px;
        font-weight: bold;
        padding-top: 60px;
        vertical-align: center;
        margin-left: 40px;

    }
    #UserPoints{
        position: absolute;
        margin-top: 95px;
        margin-left: 40px;
        font-family:Verdana;
        color: white;
        font-size: 30px;
        vertical-align: center;
    }
    #points{
        font-weight: bold;
        display: table-cell;
    }
    #text{
        padding-left: 10px;
        display: table-cell;
        font-size: small;
    }

    #detailLeft{
        margin-top: -155px;
        margin-left: 30px;
        width: 120px;
        height: 120px;
        border-radius: 100%;
    }
    #detailRight{
        vertical-align: center;
        margin-top: -120px;
        margin-left: 250px;
        width: 120px;
        height: 120px;
        border-radius: 100%;
    }
    #smallDetailLeft{
        margin-top: -85px;
        margin-left: -80px;
        width: 60px;
        height: 60px;
        border-radius: 100%;
    }
    #smallDetailRight{
        margin-top: -60px;
        margin-left: 415px;
        width: 60px;
        height: 60px;
        border-radius: 100%;
    }

</style>