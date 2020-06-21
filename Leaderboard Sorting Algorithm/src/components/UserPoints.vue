<template>
    <div id="container" v-bind:class = "compColor" >
        <div id="nameSpace" v-bind:class = "compColor">
            <div id="nameText" >{{ name }}</div>
        </div>
        <div class="spacing"/>
        <div id="points" >{{ points }} </div>
        <div id ="text" >pts</div>
    </div>
</template>

<script>
    export default {
        name: 'UserPoints',
        props: {
            name: String,
            points: Number,
            position: String,
            color: String
        },

        computed: {
            compColor: function() {
                let first = false, last = false, middle = false;
                if (this.position === "first") {
                    console.log(this.name + " is first!");
                    first = true;
                } else if (this.position === "last") {
                    console.log(this.name + " is last!");
                    last = true;
                } else {
                    console.log(this.name + " is in the middle of the pack");
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
    #container{
        display:table;
        width: 90%;
        height:54px;
        margin-left: 15px;
        margin-bottom: 10px;
        border-radius: 10px;
        border-style: solid;
        border-width: 3px;
    }
    .first{
        color: #E06E3C;
        border-color: #E06E3C;
    }
    .middle{
        color: #28847B;
        border-color: #28847B;
    }
    .last {
        color: #808080;
        border-color: #808080;
    }

    #text{
        font-family:Verdana;
        display: table-cell;
        text-align: left;
        width: 5%;
        padding-bottom: 10px;
        padding-left: 3px;
        padding-right: 3px;
        vertical-align: bottom;
        font-size: small;
    }
    #points{
        font-family:Verdana;
        vertical-align: middle;
        text-align: right;
        display: table-cell;
        width: 30%;
        font-size: 24px;
        font-weight: bold;
    }
    .spacing{
        width: auto;
        height: auto;
    }

    #nameText{
        font-family:Verdana;
        padding: 5px;
        margin: auto;
        color: white;
        font-size: 24px;
    }
    #nameSpace{
        display: flex;
        alignment: left;
        width: 70%;
        height: 54px;
        color: white;
        overflow: hidden;
    }
    .first #nameSpace{
        background: #E06E3C;
    }
    .middle #nameSpace{
        background: #28847B;
    }
    .last #nameSpace{
        background: #808080;
    }

</style>